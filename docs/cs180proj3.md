---
layout: project
title: Face Morphing and Modeling a Photo Collection
permalink: /cs180proj3/
--- 

<h1 style="text-align:center">Face Morphing and Modeling a Photo Collection</h1>
<h2 style="text-align:center">Fall 2024</h2>

<p align="center">In this project, I use affine transformations to morph some pictures in cool ways.
</p>

## Table of Contents

1. <a href="#one">Defining Correspondences</a>
2. <a href="#twoo">Computing the "Mid-Way Face"</a>
3. <a href="#three">The Morph Sequence"</a>
4. <a href="#four">The "Mean Face" of a Population</a>
5. <a href="#five">Caricatures: Extrapolating from the Mean</a>
6. <a href="#six">Bells and Whistles</a>

<br/>
<a name = "one"></a>
## 1. Defining Correspondences

In order to morph an image into another image, we first must define a set of points, or correspondences, in both images. I used <a href="https://cal-cs180.github.io/fa23/hw/proj3/tool.html">this online tool</a> to choose a set of 30-50 points between two images. Then, I computed a Delaunay triangulation on the first image's correspondences. The same triangulation was applied on the second image to ensure that the triangles matched between the images (the morph would not work properly otherwise). 

<section id="two">
<div class="column">
    <div class="row">
        <article class="proj-item-2">
            <img src="../images/180proj3/therock.jpg" alt="" />
            <br/>
            <h3>The Rock</h3>
        </article>
        <article class="proj-item-2">
            <img src="../images/180proj3/johncena.jpg" alt="" />
            <br/>
            <h3>John Cena</h3>
        </article>
    </div>
</div>
</section>

<section id="two">
<div class="column">
    <div class="row">
        <article class="proj-item-1">
            <img src="../images/180proj3/rock_cena_pts.jpg" alt="" />
            <br/>
            <h3>Correspondence points and triangulation</h3>
        </article>
    </div>
</div>
</section>

<br/>
<a name = "twoo"></a>
## 2. Computing the "Mid-Way Face"

To compute the "mid-way face" between two images, I took the average of the correspondence points between the two images to produce an average shape. Then, I warped both images to the average shape. To do this, I computed the affine transformation matrix of each triangle from each image to the average shape. Then I implemented an inverse warp using the inverse of the transformation matrix. As opposed to a forward warp, an inverse warp starts at the destination image and extracts pixel values from the starting image by warping the destination triangle coordinates. Inverse warping can be more advantageous as it does not create holes in the resulting image. 

To avoid looping over the raw pixels, I used skimage.draw.polygon to create a mask over each triangle. With this method, I was able to extract multiple pixels from the starting image at once. 

For coordinates that landed in between integers during the inverse warp, I determined the pixel value by rounding to the nearest integer. I also tried bilinear interpolation, but this ended up taking too long,and there was no significant difference between the two methods.

To cross-dissolve, I took the average pixel value between the two warped images. 

The final result is an interesting blend:

<section id="two">
<div class="column">
    <div class="row">
        <article class="proj-item-1">
            <center><img src="../images/180proj3/rockcena_mesh.png" width="500vw" alt="" /></center>
            <br/>
        </article>
    </div>
</div>
</section>

<section id="two">
<div class="column">
    <div class="row">
        <article class="proj-item-3">
            <img src="../images/180proj3/therock.jpg" alt="" />
            <br/>
            <h3>The Rock</h3>
        </article>
        <article class="proj-item-3">
            <img src="../images/180proj3/johncena.jpg" alt="" />
            <br/>
            <h3>John Cena</h3>
        </article>
        <article class="proj-item-3">
            <img src="../images/180proj3/rockcena_nearest.png" alt="" />
            <br/>
            <h3>Rock Cena</h3>
        </article>
    </div>
</div>
</section>

<br/>
<a name = "three"></a>
## 3. The Morph Sequence

To create a morph sequence between two images, I implemented the same methods as in computing the mid-way face, but with a weighted average. In code, this looked something like ((1 - coeff) * im1_pts) * (coeff * im2_pts) for the average shape, and ((1 - coeff) * im1_pixels) * (coeff * im2_pts) for the cross-dissolve. For the purposes of this project, my weighted average (coeff) was i/45, where i was the frame number in the sequence. 0/45 corresponded to the first image, and 45/45 corresponded to the second image. 

Final morph sequence between the Rock and John Cena:

<section id="two">
<div class="column">
    <div class="row">
        <article class="proj-item-1">
            <img src="../images/180proj3/rockcena.gif" width='300vw' alt="" />
            <br/>
            <h3>Rock Cena morph sequence</h3>
        </article>
    </div>
</div>
</section>

I also created a morph sequence between the Rock and a rock (the same rock from the previous project): 

<section id="two">
<div class="column">
    <div class="row">
        <article class="proj-item-1">
            <img src="../images/180proj3/rockrock2.gif" width="300vw" alt="" />
            <br/>
            <h3>Rorkrock returns</h3>
        </article>
    </div>
</div>
</section>

<br/>
<a name = "four"></a>
## 4. The "Mean Face" of a Population

In this part, I took 200 faces from the <a href="https://fei.edu.br/~cet/facedatabase.html">FEI Face Database</a>, a Brazilian face database. Using the 46 correspondences provided, I computed the average shape across all 200 faces and warped each face to the average shape. For cross-dissolve, I took the average pixel value across all warped faces.

Images 0, 1, and 2 from FEI database, warped to the average shape:
<section id="two">
<div class="column">
    <div class="row">
        <article class="proj-item-1">
            <center><img src="../images/180proj3/smile_warped.jpg" width="300vw" alt="" /></center>
            <br/>
        </article>
    </div>
</div>
</section>

Your average Brazilian smile:
<section id="two">
<div class="column">
    <div class="row">
        <article class="proj-item-1">
            <img src="../images/180proj3/smile_avg.png" width="300vw" alt="" />
        </article>
    </div>
</div>
</section>

I tried warping my face to this average Brazilian face's geometry, and vice versa. I found the result to be slightly disturbing:

<section id="two">
<div class="column">
    <div class="row">
        <article class="proj-item-2">
            <img src="../images/180proj3/me.jpg" alt="" />
            <br/>
            <h3>Me</h3>
        </article>
        <article class="proj-item-2">
            <img src="../images/180proj3/me_pts.png" alt="" />
            <br/>
            <h3>Me with correspondence points</h3>
        </article>
    </div>
</div>
</section>

<section id="two">
<div class="column">
    <div class="row">
        <article class="proj-item-2">
            <img src="../images/180proj3/smile_avg.png" alt="" />
            <br/>
            <h3>Average Brazilian face</h3>
        </article>
        <article class="proj-item-2">
            <img src="../images/180proj3/points.png" alt="" />
            <br/>
            <h3>Average Brazilian face with correspondence points</h3>
        </article>
    </div>
</div>
</section>

<section id="two">
<div class="column">
    <div class="row">
        <article class="proj-item-1">
            <center><img src="../images/180proj3/me_and_avg.png" width="700vw" alt="" /></center>
            <h3>Me warped to average face geometry, and average faced warped to my geometry</h3>
            <br/>
        </article>
    </div>
</div>
</section>

<br/>
<a name = "five"></a>
## 5. Caricatures: Extrapolating from the mean

In this part I tried extrapolating my face from the average Brazilian face, and vice versa. To do this, I computed the difference between my image's correspondence points and the mean image's correspondence points. I added this difference to my image's correspondence points, scaled by an alpha coefficient factor. To emphasize my features, I computed ((me - average)*alpha + me), and to emphasize the mean image's features, I computed ((average - me) * alpha + me). Note again that all of this is done on the correspondence points; I warped my image based on the resulting correspondence points. A higher alpha value resulted in a stronger extrapolation.

Extrapolating my features:
<section id="two">
<div class="column">
    <div class="row">
        <article class="proj-item-1">
            <center><img src="../images/180proj3/me_caricature.png" width="700vw" alt="" /></center>
            <br/>
        </article>
    </div>
</div>
</section>

Extrapolating the mean's features:
<section id="two">
<div class="column">
    <div class="row">
        <article class="proj-item-1">
            <center><img src="../images/180proj3/avg_caricature.png" width="700vw" alt="" /></center>
            <br/>
        </article>
    </div>
</div>
</section>

<a name = "six"></a>
## 6. Bells & Whistles -- Myself over the years

I collected images of myself from 2005 to 2024, aligned them, and added correspondance points following the FEI face database's point assignment pattern. Then I created a morph sequence across all of these images. 

<center><iframe width="50%"
src="https://www.youtube.com/embed/yDkoAeu3T2o"
title="YouTube video player"
frameborder="0"
allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share"
allowfullscreen></iframe>

Time it took: 10.2 hours
</center>