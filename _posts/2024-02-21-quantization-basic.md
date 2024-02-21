---
title: Quantization Basic
tags: [Quantization, Math]
style: danger
color: info
description: Basic knowledge of quantization.
---
[Huggingface Docs](https://huggingface.co/docs/optimum/concept_guides/quantization)

# Quantization

Quantization is a technique to reduce the computational and memory costs of running inference by representing the weights and activations with low-precision data types like 8-bit integer (`int8`) instead of the usual 32-bit floating point (`float32`).

## Theory

Common lower precision data types

| data type | accumulation data type |
| :-------: | :--------------------: |
|  float16  |        float16        |
| bfloat16 |        float32        |
|   int16   |         int32         |
|   int8   |         int32         |

The accumulation data type specifies the type of the result of accumulating (adding, multiplying, etc) values of the data type

## Quantization

The two most common quantization cases

- float32 -> float16
- float32 -> int8

### Quantization to float16

**Question:**

* Is my operation sensitive to lower precision?
  * For instance the value of epsilon in `LayerNorm` is usually very small (~ `1e-12`), but the smallest representable value in `float16` is ~ `6e-5`, this can cause `NaN` issues.
  * The same applies for big values.

### Quantization to int8 (`affine quantization sheme`)

Only 256 values can be represented in `int8`, while `float32` can represent a very wide range of values.

The idea is to find the best way to **project our range `[a, b]` of `float32` values to the `int8` space**.

Consider a float `x` in `[a, b]`:

$$
x = S * (x_q - Z)
$$

- S: scale, a positive `float32`
- Z: zero-point, it is the `int8` value corresponding to the value 0 in the float32 realm

Thus the quantizaed value can be computed as follows:

$$
x_q = \mathrm{round}(x/S + Z)
$$

And float32 values outside of the [a, b] range are clipped to the closest representable value:

$$
x_q = \mathrm{clip}(\mathrm{round}(x/S + Z), \mathrm{round}(a/S + Z), \mathrm{round}(b/S + Z))
$$

### Symmetric and Affine Quantization Schemes

#### speedup

- A common special case of this scheme is the `symmetric quantization scheme`, where we consider a symmetric range of float values `[-a, a]`.
  - In this case the integer space is usally `[-127, 127]`. (meaning that the -128 is opted out of the regular `[-128, 127]` signed `int8` range.)
  - The reason being that having both ranges symmetric allows to have Z = 0.
  - While one value out of the 256 representable values is lost, it can provide a speedup since *a lot of addition operations can be skipped*.

### Calibration

Calibration is the step during quantization where the `float32` ranges are computed.

For weights it is quite easy since the actual range is known at `quantization-time`.

But it is less clear for activations, and different approaches exist:

- Post training **dynamic quantization**
  - the range for each activation is computed on the fly at runtime
  - it can be a bit slower than static quantization
    - the overhead introduced by computing the range each time
- Post training **static quantization**
  - the range for each activation is computed in advance at quantization-time
    - Observers are put on activations to record their values.
    - A certain number of forward passes on a calibration dataset is done (around 200 examples is enough).
    - The ranges for each computation are computed according to some calibration technique.
- **Quantization aware training**
  - the range for each activation is computed at training-time
    - “fake quantize” operators are used instead of observers
    - record values
    - simulate the error induced by quantization to let the model adapt to it

#### Calibration techniques
- Min-max: `[min observed value, max observed value]`
- Moving average min-max: `[moving average min observed value, moving average max observed value]`
- Histogram: records a histogram of values along with min and max values, then chooses according to some criterion
  - Entropy: the one minimizing the error between the full-precision and the quantized data.
  - Mean Square Error: the one minimizing the mean square error between the full-precision and the quantized data.
  - Percentile: using a given percentile value `p` on the observed values. The idea is to try to have `p%` of the observed values in the computed range.

## How do machines represent numbers?
### Real numbers representation
- Fixed-point: there are fixed number of digits reserved for representing the integer part and the fractional part
- Floating-point: the number of digits for representing the integer and the fractional parts can vary
  - The sign bit: this is the bit specifying the sign of the number
  - The exponent part
    - 5 bits in float16
    - 8 bits in bfloat16
    - 8 bits in float32
    - 11 bits in float64
  - The mantissa
    - 11 bits in float16 (10 explictly stored)
    - 8 bits in bfloat16 (7 explicitly stored)
    - 24 bits in float32 (23 explictly stored)
    - 53 bits in float64 (52 explictly stored)

For a real number `x` we have
$$
x = sign \times mantissa \times (2^{exponent})
$$

# Going Further
Lei Mao's blog about [Quantization for Neural Networks](https://leimao.github.io/article/Neural-Networks-Quantization/).
