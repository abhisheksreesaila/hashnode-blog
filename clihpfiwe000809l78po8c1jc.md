---
title: "Computer Vision Basics"
datePublished: Sun Jun 04 2023 17:36:07 GMT+0000 (Coordinated Universal Time)
cuid: clihpfiwe000809l78po8c1jc
slug: computer-vision-basics
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1700599157543/55187ba1-d373-4dfe-8ea7-7c9301e28a0e.png
ogImage: https://cdn.hashnode.com/res/hashnode/image/upload/v1700599167476/8c60d6ec-9072-477a-9f0d-d8537f89eb34.png
tags: computer-vision

---

![](https://abhisheksreesaila.github.io/blog/images/stereo/pinhole.png align="left")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1689660509277/674fd076-6753-4a7e-9e8c-dbe8e7ead87c.png align="left")

Before we go explain this in detail let's clarify the terms

* The object (flower) is said to be **in the** **world** and its position is given by **world coordinates**
    
* Now imagine The light rays from the object pass through a pinhole and get projected on a wall. The wall is the **IMAGE plane** and the hole it passes through is called the **PINHOLE**. The camera is placed at the pinhole. The camera is said to be in the **CAMERA plane**
    
* The distance between the camera placed at the pinhole and the wall (image plane) is called **FOCAL LENGTH**
    

## So what is the point of all this definition?

* > <mark>Our goal is to take the 3D coordinates (x,y,z) and first </mark> **<mark>TRANSFORM</mark>** <mark>them to the camera plane (3D) and </mark> **<mark>PROJECT</mark>** <mark>them to the image plane (2D).</mark>
    

In other words, we move from WORLD =&gt; CAMERA =&gt; IMAGE. This is called **Forward Imaging Model.** The projection from the Camera to the image plane is called **Perspective Projection**. We can write the forward imaging model as

\\( [x_w, y_w, z_w] => [x_c, y_c, z_c] => [x_i, y_i] \\)

# Perspective Imaging with Pinhole

![](https://abhisheksreesaila.github.io/blog/images/stereo/similartri.png align="left")

The above diagram is the same as the Pinhole diagram with labels removed for explaining the geometry. The yellow and blue triangles are similar. By the property of similar triangles (see below note), we have

\\(  \frac{r_i}{f} = \frac{r_o}{z} \\)

Where \\(r\_o = (x\_o, y\_o, z\_o )\\) and \\( r\_i = (x\_i, y\_i, f)\\)

where \\(r\_i\\) and \\(  r\_o \\) are vectors describing the point on the image and world respectively. Hence we can split and write the equation as

$$x_i/f = x_o/z$$

$$y_i/f = y_o/z$$

This is the <mark>standard pinhole camera equation</mark>. The equation has a few interesting properties.

## Pinhole Properties

1. The straight line remains straight in the image plane
    
2. <mark>Magnification is inversely proportional to depth <latex-inline-node data-content=" z_o ">z_o </latex-inline-node>.</mark> In other words, The further you are from the camera, the smaller you appear. Now this is obvious since we all use cameras today. But take a look at the parallel lines intersecting for a railway track. The distance between the parallel tracks becomes so small in the image plane that it looks like an intersection though, in reality, we know it's parallel throughout. <mark>The point of intersection is called the </mark> **<mark>Vanishing Point</mark>**
    
3. Since a pinhole is a tiny hole letting the light inside the camera capture the images, for a large object it will take a few seconds before the image is captured. Imagine clicking a camera and waiting 5 seconds to see the image!! To improve this, we substitute pinhole with LENS.
    

## Pinhole vs Lens

We won't cover LENS and its properties, but just keep in mind, LENS is just a "pinhole" made larger. That's a simplistic view of it. Obviously with more area to capture light, now the wait time to capture the image is reduced, but the light is scattered now, and hence causes distortions in your image. Apart from the focal length we introduce **distortion coefficients** as one of the Intrinsic Parameters. All modern cameras use sophisticated lenses.

See below

![](https://abhisheksreesaila.github.io/blog/images/stereo/lens-camera.png align="left")

---

# References

[OpenCV Calibration](https://docs.opencv.org/3.4.3/d9/d0c/group__calib3d.html)

[Geometry of Image Formation](https://learnopencv.com/camera-calibration-using-opencv/)

[Pinhole basics](https://www.youtube.com/watch?v=_EhY31MSbNM&t=194s)