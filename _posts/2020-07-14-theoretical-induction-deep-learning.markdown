---
layout: post
title: "Implement a Deep Learning Framework in Pure Python"
subtitle: 'I: Theoretical Induction.'
author: "Xiaozhe Yao"
mathjax: true
header-img: "img/posts/cnn.jpg"
tags:
    - Machine Learning
---

In the following induction, we will by default define the following
symbols:

-   $w_i$ and $b_i$: the weight and bias of the $i$-th layer in a neural
    network.

-   $x_i$: the input to the $i$-th layer in a neural network.

-   $y_i$: the output of the $i$-th layer. On the contrary, if there is
    corresponding ground truth label, it is notated as $\hat{y}_i$.

-   $l$: the value of loss function. It can be mean square error, cross entropy
    loss or any other form.

There are two fundamental observations in back propagation:

-   In the $i$-th layer, we always know the gradient of the loss with
    respects to the output of $i$-th layer. That means, in $i$-th layer,
    $\frac{\partial l}{\partial y_i}$ is given.

-   Since we know that the output of $(i-1)$-th layer is the input of
    $i$-th layer, when performing backward pass, we have
    $\frac{\partial{l}}{\partial{x_i}}=\frac{\partial{l}}{\partial{y_{i-1}}}$.

## Fully Connected Layers

In forward pass, the output of fully connected layers is simple:
$y_i=w_i \times x_i + b_i$.

Then in order to know how $w$ changes will affect the loss, we need to
calculate $\frac{\partial{l}}{\partial{w_i}}$. By using the chain rule,
we have
$\frac{\partial{l}}{\partial{w_i}}=\frac{\partial{l}}{y_i}\frac{\partial{y_i}}{\partial{w_i}}=\frac{\partial{l}}{y_i}x_i$,
and
$\frac{\partial{l}}{\partial{b_i}}=\frac{\partial{l}}{y_i}\frac{\partial{y_i}}{\partial{b_i}}=\frac{\partial{l}}{y_i}$.
We can then successfully update our weight and bias in this layer.

After updating the weight and bias in $i$-th layer, we also need to pass
the gradient of loss with respect to the input to the previous layer. So
we need to compute the gradient that the $i$-th layer passed to previous
layer by
$\frac{\partial{l}}{\partial{x_i}}=\frac{\partial{l}}{\partial{y_i}}\frac{\partial{y_i}}{\partial{x_i}}=\frac{\partial{l}}{\partial{y_i}}w_i$.

## ReLu

The purpose of using activation functions is to bring some non-linearity
into the deep neural networks, so that the networks can fit the real
world. One of the most popular activation function is the **re**ctifier
**l**inear **u**nit (ReLu).

The function is defined as $f(x)=max(0,x)$. Thus the forward pass is
simple: $y_i=max(0, x_i)$.

In the ReLu function, we do not have any weight or bias to update. Hence
we only need to compute the gradient to previous layer. We have
$\frac{\partial{l}}{\partial{x_i}}=\frac{\partial{l}}{\partial{y_i}}\frac{\partial{y_i}}{\partial{x_i}}$.
Then we have:

$$\frac{\partial{l}}{\partial{x_i}}=
  \begin{cases}
    $0$ & \text{$x_i<0$}  \\
    \frac{\partial{l}}{\partial{y_i}} & \text{$x_i>0$} \\
    undefined & \text{$x_i=0$}
  \end{cases}$$

We see that the derivative is not defined at the point $x_i=0$, but when
computing, we can set it to be $0$, or $1$, or any other values between.

## Softmax

Softmax is such a function that takes the output of the fully connected
layers, and turn them into the probability. Formally, it takes an $n$-d
vector, and normalizes it to $n$ probabilities proportional to the
exponentials of the input number. It is defined as
$$f(x)=\frac{e^{x_i}}{\sum e^{x_j}}$$, where $x_i$ is the $i$-th input
number.

We can then compute the derivative by using the quotient rule (if
$f(x)=\frac{g(x)}{h(x)}$, then
$f'(x)=\frac{g'(x)h(x)-g(x)h(x)}{h^2(x)}$). In our case, we have
$g_i=e^{x_i}$ and $h_i=\sum e^{x_j}$. Then we have
$\frac{\partial g_i}{x_j}=e^{x_i} \: (i=j)$ or $0 \: (i\neq j)$. For
$h_i$, no matter the relation between $i$ and $j$, the derivative will
always be $e^{x_i}$.

Thus we have:

When $i=j$,
$$\frac{\partial f}{\partial x_i}=\frac{e^{x_i}\sum e^{x_j}-e^{x_i}e^{x_j}}{(\sum e^{x_j})^2}=\frac{e^{x_i}}{\sum{e^{x_j}}}\times \frac{(\sum e^{x_i} - e^{x_i})}{\sum{e^{x_j}}} = f(x_i)(1-f(x_i))$$

When $i\neq j$,
$$\frac{\partial f}{\partial x_i}=\frac{0-e^{x_i}e^{x_j}}{(\sum e^{x_j})^2}=-\frac{e^{x_i}}{\sum e^{x_j}}\times \frac{e^{x_j}}{\sum e^{x_j}}=-f(x_i)f(x_j)$$

## Mean Square Loss

The mean square error is defined as
$l = \frac{1}{n}\sum (y_i-\hat{y}^i)^2$. Since this is the last
derivative we need to compute, we will only need to compute
$\frac{\partial l}{\partial y_i}$. Let $g(y_i)=y_i-\hat{y_i}$, then
$\frac{\partial g}{\partial y_i}=1$.

$$\frac{\partial l}{\partial y_i} = \frac{\partial l}{\partial g}\times
\frac{\partial g}{{\partial y_i}} = \frac{2}{n}(y_i-\hat{y_i})$$

## Cross Entropy Loss

The cross-entropy loss is defined as $l=-\sum_i^n \hat{y_i}log(p(y_i))$
where $p(y_i)$ is the probability of the output number, i.e. we usually
use cross-entropy loss after a softmax layer. By this nature, we could
actually compute the derivative of cross-entropy loss with respect to
the original output $y_i$ rather than $p(y_i)$.

Then we have:

$$\frac{\partial l}{\partial y_i}=- \sum_j \hat{y_j} \frac{\partial log(p(y_j))}{\partial y_i} = -\sum_j \hat{y_j} \frac{1}{p(y_j)}\frac{\partial p(y_j)}{\partial y_i}$$

Then as we know there will be a $k=i$ such that
$\frac{p(y_k)}{\partial y_i}=p(y_j)(1-p(y_j))$, and for other $k\neq i$,
we have $\frac{p(y_k)}{\partial y_i}=-p(y_j)p(y_i)$.

Then we have: 

$$\begin{array}{l}
-\sum_j \hat{y_j} \frac{1}{p(y_j)}\frac{\partial p(y_j)}{\partial y_i} \\ 
= (-y_i)(1-p(y_i))-\sum_{j\neq i} \hat{y_j} \frac{1}{p(y_j)}p(y_j)p(y_i) \\
= -y_i + p(y_i)y_i + \sum_{j\neq i}y_jp(y_i) \\ 
= -y_i + p(y_i)\sum_{j\neq i} y_j \\ 
= -y_i + p(y_i)\sum_{j}p(y_j) \\
= p(y_i) - y_i
\end{array}$$

The form is very elegant, and easy to compute. Therefore we usually hide
the computational process of the derivative of softmax in the
computation of cross entropy loss.
