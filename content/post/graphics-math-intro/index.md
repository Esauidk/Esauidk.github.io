---
title: "3D Graphics Intro"
author: Esau Abraham
date: 2022-10-16
draft: false
categories: 
    - Graphics
    - Math
description: My notes on intro graphics material
image: https://cdn5.vectorstock.com/i/1000x1000/47/14/3d-coordinate-axis-vector-7814714.jpg
katex: true
markup: 'mmark'
---

# Overview & Affine Transformation
## 3D Rotation
- Cos(theta) in the matrix scaling spots
- Sin(theta) and -Sin(theta) in the matrix sheering spots

**2D Rotation**
$$
\begin{bmatrix}
Cos(ϴ) & -Sin(ϴ) \\
Sin(ϴ) & Cos(ϴ)
\end{bmatrix}$$


**3D Rotation**
$$
\begin{bmatrix}
Cos(ϴ) & -Sin(ϴ) & 0  \\
Sin(ϴ) & Cos(ϴ) & 0 \\
0 & 0 & 1
\end{bmatrix}
$$


## Homogeneous Coordinates
Have an extra collumn in the transformation matrix to represent translation

**2D Translation**
- Move to a 3D dimension and make the third collumn in the matrix a translation vector
$$
\begin{bmatrix}
x+1  \\
y  \\
0
\end{bmatrix}
=
\begin{bmatrix}
x+1  \\
y  \\
\end{bmatrix}
=
\begin{bmatrix}
1 & 0 & 1 \\
0 & 1 & 0 \\
0 & 0 & 0
\end{bmatrix}
*
\begin{bmatrix}
x  \\
y  \\
1
\end{bmatrix}
$$
