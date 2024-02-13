---
title: The Matrix Calculus You Need For Deep Learning
tags: [Notes, Math, Interview]
style: danger
color: info
description: Notes of The Matrix Calculus You Need For Deep Learning
---
# Introduction

This article walks through the derivation of some important rules for computing partial derivatives with respect to vectors, particularly those useful for training neural networks.

# Matrix Calculus

## Generalization of the Jacobian

$$
\frac {\partial \textbf{y}}{\partial \textbf{x}}
= 
\begin{bmatrix} 
\nabla f_1(\textbf{x}) \\
\nabla f_2(\textbf{x}) \\
\cdots \\
\nabla f_m(\textbf{x})
\end{bmatrix}
=
\begin{bmatrix}
\frac{\partial}{\partial \textbf{x}} f_1(\textbf{x}) \\
\frac{\partial}{\partial \textbf{x}} f_2(\textbf{x}) \\
\cdots \\
\frac{\partial}{\partial \textbf{x}} f_m(\textbf{x}) \\
\end{bmatrix}
=
\begin{bmatrix}
\frac{\partial}{\partial x_1} f_1(\textbf{x}) & \frac{\partial}{\partial x_2} f_1(\textbf{x}) & \cdots & \frac{\partial}{\partial x_n} f_1(\textbf{x}) \\
\frac{\partial}{\partial x_1} f_2(\textbf{x}) & \frac{\partial}{\partial x_2} f_2(\textbf{x}) & \cdots & \frac{\partial}{\partial x_n} f_2(\textbf{x}) \\
\cdots \\
\frac{\partial}{\partial x_1} f_m(\textbf{x}) & \frac{\partial}{\partial x_2} f_m(\textbf{x}) & \cdots & \frac{\partial}{\partial x_n} f_m(\textbf{x}) \\
\end{bmatrix}
$$

## Derivatives of vector element-wise binary operators

$$
y = f(w) \circ g(x)
$$

$$
\begin{bmatrix}
y_1 \\
y_2 \\
... \\
y_n
\end{bmatrix}
=
\begin{bmatrix}
f_1(w) \circ g_1(x) \\
f_2(w) \circ g_2(x) \\
... \\
f_n(w) \circ g_n(x)
\end{bmatrix}
$$

As $\frac{\partial}{\partial w_j} (f_i(x_i) \circ g_i(x_i)) = 0$ where $j \neq i$, thus $\frac{\partial}{\partial w_j} (f_i(x) \circ g_i(x)) = 0$ where $j \neq i$.

Thus we have:

$$
\frac{\partial y}{\partial w} = diag (\frac{\partial}{\partial w_1}(f_1(w_1) \circ g_1(x_1)), \cdots , \frac{\partial}{\partial w_n}(f_n(w_n) \circ g_n(x_n)))
$$

Common element-wise binary operations:

- $\frac{\partial (w+x)}{\partial w} = diag(1) = I$
- $\frac{\partial (w-x)}{\partial w} = diag(1) = I$
- $\frac{\partial (w \otimes x)}{\partial w} = diag(\cdots, \frac{\partial (w_i \times x_i)}{\partial w_i}, \cdots) = diag(x)$
- $\frac{\partial (w \oslash x)}{\partial w} = diag(\cdots, \frac{\partial (w_i / x_i)}{\partial w_i}, \cdots) = diag(\cdots, \frac{1}{x_i}, \cdots)$

## Derivatives involving scalar expansion

Adding scalar z to vector x, $y = x + z$, is really $y = f(x) + g(z)$ where $f(x) = x$ and $g(z) = \vec{1}z$

$$
\frac{\partial}{\partial x_i} (f_i(x_i) + g_i(z)) = \frac{x_i}{\partial x_i} + \frac{z}{\partial x_i} = 1
$$

So $\frac{\partial (x + z)}{\partial x} = I$, $\frac{\partial (x + z)}{\partial z} = \vec{1}$

$$
\frac{\partial}{\partial x_i} (f_i(x_i) \otimes g_i(z)) = z
$$

So $\frac{\partial (xz)}{\partial x} = diag(\vec{1}z) = Iz$

$$
\frac{\partial}{\partial z} (f_i(x_i) \otimes g_i(z)) = x_i \frac{\partial z}{\partial z} + z \frac{\partial x_1}{\partial z} = x_i
$$

So $\frac{\partial (xz)}{\partial z} = x$

## Vector sum reduction

Let $y = \mathrm{sum}(f(x)) = \Sigma_{i=1}^n f_i(x)$

$$
\frac{\partial y}{\partial x}
=
\begin{bmatrix}
    \frac{\partial y}{\partial x_1}, \cdots, \frac{\partial y}{\partial x_n} \\
\end{bmatrix}
\\=
\begin{bmatrix}
    \frac{\partial}{\partial x_1} \Sigma_{i} f_i(x), \cdots, \frac{\partial}{\partial x_n} \Sigma_{i} f_i(x) \\
\end{bmatrix}
\\=
\begin{bmatrix}
    \Sigma_{i} \frac{\partial f_i(x)}{\partial x_1}, \cdots, \Sigma_{i} \frac{\partial f_i(x)}{\partial x_n}  \\
\end{bmatrix}
$$

If $y = sum(x)$:

$$
\nabla y = 
\begin{bmatrix}
    \frac{\partial x_1}{\partial x_1}, \cdots, \frac{\partial x_n}{\partial x_n} \\
\end{bmatrix}
= \vec{1}^T
$$

If $y = sum(\boldsymbol{x}z)$ then $f_i(x, z) = x_iz$

$$
\frac{\partial y}{\partial x} = 
\begin{bmatrix}
    z, \cdots, z \\
\end{bmatrix}
$$

$$
\frac{\partial y}{\partial z} = \Sigma_i x_i = sum(x)
$$

## The Chain Rules

### Single-variable chain rule

$$
\frac{dy}{dx} = \frac{dy}{du} \frac{du}{dx}
$$

### Single-variable total-derivative chain rule

$$
\frac{\partial f(x,u_1, \cdots, u_n)}{\partial x} = \frac{\partial f}{\partial x} + \Sigma_{i=1}^n \frac{\partial f}{\partial u_i} \frac{\partial u_i}{\partial x}
$$

### Vector chain rule

$$
\frac{\partial}{\partial \boldsymbol{x}} \boldsymbol{f}(g(\boldsymbol{x})) = \frac{\partial \boldsymbol{f}}{\partial g} \frac{\partial g}{\partial x}
\\ = 
\begin{bmatrix}
    \frac{\partial f_1}{\partial g_1} \frac{\partial f_1}{\partial g_2} \cdots \frac{\partial f_1}{\partial g_k} \\
    \frac{\partial f_2}{\partial g_1} \frac{\partial f_2}{\partial g_2} \cdots \frac{\partial f_2}{\partial g_k} \\
    \cdots \\
    \frac{\partial f_m}{\partial g_1} \frac{\partial f_m}{\partial g_2} \cdots \frac{\partial f_m}{\partial g_k} 
\end{bmatrix}
\begin{bmatrix}
    \frac{\partial g_1}{\partial x_1} \frac{\partial g_1}{\partial x_2} \cdots \frac{\partial g_1}{\partial x_n} \\
    \frac{\partial g_2}{\partial x_1} \frac{\partial g_2}{\partial x_2} \cdots \frac{\partial g_2}{\partial x_n} \\
    \cdots \\
    \frac{\partial g_k}{\partial x_1} \frac{\partial g_k}{\partial x_2} \cdots \frac{\partial g_k}{\partial x_n} 
\end{bmatrix}
$$

where $m = |f|, n = |x|, k = |g|$

As we saw in a previous section, element-wise operations on vectors w and x yield diagonal matrices. The same thing happens here when $f_i$ is purely a function of $g_i$ and $g_i$ is purely a function of $x_i$. In this situation, the vector chain rule simplifies to:

$$
\frac{\partial}{\partial \boldsymbol{x}} \boldsymbol{f}(g(x)) = diag(\frac{\partial f_i}{\partial g_i} \frac{\partial g_i}{\partial x_i})
$$

## The gradient of neuron activation

$$
activation(\boldsymbol{x}) = \mathrm{max} (0, \boldsymbol{w} \cdot \boldsymbol{x} + b)
$$

The dot product $\boldsymbol{w} \cdot \boldsymbol{x}$ is $\Sigma_i^n (w_i x_i) = sum(w \otimes x)$
Let $u = w \otimes x$, $y = sum(u)$

$$
\frac{\partial u }{\partial w} = \frac{\partial}{\partial w} (w \otimes x) = diag(x)
$$

$$
\frac{\partial y}{\partial u} =\frac{\partial}{\partial u} sum(u) = \vec{1}^T
$$

Chain Rule:

$$
\frac{\partial y}{\partial w} = \vec{1}^T diag(x) = x^T
$$

$$
\frac{\partial y}{\partial b} = 1
$$

Then calculate $max(0, z)$, $z = y +b = \boldsymbol{w} \cdot \boldsymbol{x} + b$

$$
\frac{\partial}{\partial z} max(0, z) = 
\begin{cases}
    0 , z \leq 0\\
    \frac{\partial z}{\partial z} = 1 , z > 0\\
\end{cases}
$$

---

when x is a vector:

$$
max(0, x) = 
\begin{bmatrix}
    max(0,x_1)\\
    \cdots \\
    max(0,x_n)
\end{bmatrix}
$$

The derivative of the broadcast version:

$$
\frac{\partial}{\partial x_i} max(0, x_i) = 
\begin{cases}
    0 , x_i \leq 0\\
    \frac{\partial x_i}{\partial x_i} = 1 , x_i > 0\\
\end{cases}
$$

$$
\frac{\partial}{\partial x}max(0, x) = 
\begin{bmatrix}
    \frac{\partial}{\partial x_1}max(0,x_1)\\
    \cdots \\
    \frac{\partial}{\partial x_n}max(0,x_n)
\end{bmatrix}
$$

---

Chain Rule:

$$
\frac{\partial activation}{\partial w} = 
\begin{cases}
    0 \frac{\partial z}{\partial w} = \vec{0}^T, z \leq 0\\
    1 \frac{\partial z}{\partial w} = x^T, z > 0\\
\end{cases}
$$

That is:
$$
\frac{\partial activation}{\partial w} = 
\begin{cases}
    \vec{0}^T, w \cdot x + b \leq 0\\
    x^T, w \cdot x + b > 0\\
\end{cases}
$$

the derivative of the neuron activation with respect to b:
$$
\frac{\partial activation}{\partial b} = 
\begin{cases}
    0 \frac{\partial z}{\partial b} = 0, w \cdot x +b \leq 0\\
    1 \frac{\partial z}{\partial b} = 1, w \cdot x +b > 0\\
\end{cases}
$$
## The gradient of the neural network loss function

Assume the cost equation is:
$$
C(w, b, X, y) = \frac{1}{N} \Sigma_{i=1}^N (y_i - activatation(x_i))^2 = \frac{1}{N} \Sigma_{i=1}^N (y_i - max(0, w \cdot x_i +b))^2
$$

### The gradient with respect to the weights
Let $v = y - u$, $u = max(0, w \cdot x + b)$


$$
\frac{\partial C(v)}{\partial w} = \frac{2}{N} \Sigma_{i=1}^N v \frac{\partial v}{\partial w}
\\ = \frac{2}{N} \Sigma_{i=1}^N
\begin{cases}
    \vec{0}^T, w \cdot x +b \leq 0 \\
    -vx_i^T = -(y_i - u)x_i^T = (w \cdot x + b -y_i) x_i^T, w \cdot x +b > 0
\end{cases}
$$

### The gradient with respect to the bias
$$
\frac{\partial C(v)}{\partial b} = \frac{2}{N} \Sigma_{i=1}^N v \frac{\partial v}{\partial b}
\\ = \frac{2}{N} \Sigma_{i=1}^N
\begin{cases}
    0, w \cdot x +b \leq 0 \\
    -v = w \cdot x + b -y_i, w \cdot x +b > 0
\end{cases}
$$