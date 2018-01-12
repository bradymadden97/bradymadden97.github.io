---
title: "Go Quads: Algorithmic Art with Go"
date: 2018-01-11T21:10:05-05:00

featuredImage: ""
categories: []
tags: ['Projects', 'Go', 'Art', 'Quad heaps', 'Open source']
author: "Brady Madden"
---
Go-quads was a project I started after finding a similar algorithmic art program online, by <a class="projectlink" target="_blank" href="https://github.com/fogleman/Quads">@fogleman</a>. I loved the idea of a program being able to create something that looked like art, algorithmically, and was looking for a way to replicate it myself.

<!--more-->

<p><div style="width:250px;margin:auto"><img style="width:250px;" src="/images/goquads1.jpg"/></div></p>

I decided to use Go as the language after being exposed to it for the first time at work, and was hoping to learn more. The project definitely had its fair share of challenges, and I worked through a few different iterations in order to make it as efficient as I could.

The general process behind the art is to take in an image and calculate the average pixel color across the entire image. That color is replaces the image area, acting as if the image was severely blurred. The algorithm iterates by splitting the original image into 4 equal quadrants and calculating each quadrant's average pixel color, then replacing that quadrant's image with the color. 

<div class="image-holder">
<img style="width:250px;margin-right:10px" src="/images/goquads.gif"/>
<img style="width:250px;" src="/images/goquads4.jpg"/>
</div>

Once the total image has at least 4 quadrants, the error between the average pixel color and the actual pixel values is calculated for each quadrant and the quadrant with the largest error is quad-split again.

To keep track of the current quadrant with the largest pixel error I stored each image struct in a heap. The image structs hold data about the dimensions of the quadrant image, the average pixel color, and the upper-left point of the quadrant in relation to the main image. 

In addition, each image struct contains a histogram of the RGBA values of each pixel in the quadrant. Storing this array of integers performed faster than storing actual sub-images and extracting pixel values each iteration.

The total quadrant pixel error is derived by using the following formula across across the quadrant pixel histogram:

$$\sum_{i=0}^p (r_i - \overline{r})^2 + (g_i - \overline{g})^2 + (b_i - \overline{b})^2 $$

where $p$ is the number of pixels in the quadrant, $\overline{r}$, $\overline{g}$, and $\overline{b}$ are the average RGB values of the quadrant.

After each iteration, the new quadrants are superimposed over the current main image at the correct position corresponding to the upper-left point stored in the quadrant image struct.

Go-quads is compiled as an executable program that can be run with flags specifying certain parameters such as the number of iterations to run the quad-splitting algorithm, border, and background colors.

<p><div style="width:250px;margin:auto"><img style="width:250px;" src="/images/goquads3.jpg"/></div></p>

An additional feature is the ability to display the quadrants as circles instead of squares. Instead of drawing an entire quadrant with one average pixel color, the pixels in the quadrant will be given one of two designations - inside the radius or outside the radius. Pixels inside the radius will be colored with the average pixel color for that quadrant and pixels outside the radius will be colored with the background color.

This project produced some really interesting images and was definitely a good learning experience with Go and image processing. Feel free to check out the source code on <a href="https://github.com/bradymadden97/go-quads" target="_blank">Github</a>!

<p><div style="width:250px;margin:auto"><img style="width:250px;border-radius:50%;" src="/images/goquads2.jpg"/></div></p>


