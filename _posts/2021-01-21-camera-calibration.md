---
layout: post
title: "相机标定法"
subtitle: '真实世界中的物体是如何变换到相机图片的'
author: "Xiaozhe Yao"
header-style: text
tags:
  - Machine Vision
---

当你拿起相机拍照时，有没有想过相机是如何把真实世界中的物体转变为一张相片？

这一过程可以分为三步：真实世界的坐标 $P_W$ 到以相机为原点的坐标 $P_C$，相机坐标 $P_C$ 到像的坐标 $(x,y)$ 和像 $(x,y)$ 到最终图片 $(u,v)$。

## 世界坐标 $P_W$ 到相机坐标 $P_C$

这一过程可以通过刚体运动来建模，即：对于同一个物体来说，它从世界坐标转换到相机坐标的过程是一个平移+旋转的过程（没有大小的变换）。所以，我们可以得到如下的结论
$$
\begin{bmatrix}
X_C \\
Y_C \\
Z_C
\end{bmatrix}=\begin{bmatrix}
r_{11} & r_{12} & r_{13} & t_1\\
r_{21} & r_{22} & r_{23} & t_2\\
r_{31} & r_{32} & r_{33} & t_3\\
\end{bmatrix}\begin{bmatrix}
X_w \\
Y_w \\
Z_w \\
1
\end{bmatrix}
$$
我们用 $R$ 来表示旋转的矩阵，$T$ 表示平移的向量。之后这一变换就可以写作
$$
\begin{bmatrix}
X_C \\
Y_C \\
Z_C
\end{bmatrix}=\begin{bmatrix}
R | T
\end{bmatrix}\begin{bmatrix}
X_w \\
Y_w \\
Z_w \\
1
\end{bmatrix}
$$

## 相机坐标 $P_C$ 到像的坐标 $(x,y)$

简单起见，我们先考虑一个 $Y_C=0$ 的点，即假设我们有一个点 $P_C=[X_C,0,Z_C]$。那么它与像的关系如下图所示：

![image-20210128112133948](https://i.loli.net/2021/01/28/UdXns96C2ObMiPt.png)

在上图中，$f$为相机的焦距，$C$为相机的中心，$O$ 为相机镜片的中心，$x$ 为物体在像上的高度，$X_C$ 为物体在相机坐标中的高度。根据相似三角形，我们可以得到：
$$
\frac{x}{f}=\frac{X_C}{Z_C}
$$
类似地，对于$y$，我们也可以得到同样的结果：
$$
\frac{y}{f}=\frac{Y_C}{Z_C}
$$
简单变换后可以得到
$$
x=f\frac{X_C}{Z_C} \\
y=f\frac{Y_C}{Z_C}
$$

## 像的坐标 $(x,y)$ 到图片的坐标 $(u,v)$

像和最终的相片并不一定是同样的大小，甚至都是不同的单位（如cm和px），因此我们需要把像的坐标转换到图片的坐标。

我们假设 $O=(u_0,v_0)$ 是相机镜片中心在最终图片上的坐标，$k_u, k_v$是像的大小到最终图片的大小的系数（分别在$x$轴和$y$轴上的）。那我们就有
$$
u=u_0+k_ux=u_0+k_uf\frac{X_C}{Z_C} \\
v=v_0+k_vy=v_0+k_vf\frac{Y_C}{Z_C}
$$
为了简便，我们令 $a_u=k_uf, a_v=k_vf$，即$a_u,a_v$是用像素表示的焦距。再令
$$
K=\begin{bmatrix}
a_u & 0 & u_0 \\
0 & a_v & v_0 \\
0 & 0 & 1\\
\end{bmatrix}
$$
则以上的全过程可以表示为
$$
K=\begin{bmatrix}
u \\
v \\
1\\
\end{bmatrix}=K[R|T]=K[R|T]\begin{bmatrix}
X_W \\
Y_W \\
Z_W \\
1
\end{bmatrix}
$$
其中，$R$ 和 $T$ 不是相机本身的参数 (extrinsic parameters)，可以想像它和你手持相机的方式有关。而 $K$ 是相机本身的参数 (intrinsic parameters)，即焦距和相机镜片到图片上的坐标。确定 $K, R, T$ 的过程即被称作相机标定 (camera calibration)。

## 相机标定法



