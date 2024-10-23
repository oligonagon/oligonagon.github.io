---
layout: project
title: Image Warping and Mosaicing
permalink: /cs180proj4/
--- 

<h1 style="text-align:center">Image Warping and Mosaicing</h1>
<h2 style="text-align:center">Fall 2024</h2>

<p align="center">In this project, I use homography to rectify images and make basic panoramas. 
</p>

<center><h2> Table of Contents </h2>

1. <a href="#one">Taking Pictures</a><br/>
2. <a href="#twoo">Recovering Homographies</a><br/>
3. <a href="#three">Image Rectification</a><br/>
4. <a href="#four">Image Blending</a><br/></center>

<br/>
<a name = "one"></a>

## 1. Taking Pictures

Below are the images I used for this project.
<section id="two">
<div class="column">
    <div class="row">
        <article class="proj-item-2">
            <h3> Diagram of the Campanile Bells </h3>
            <img src="../images/180proj4/rectification.jpg" width="100px" alt="" />
            <br/>
        </article>
        <article class="proj-item-2">
            <h3> Max Occupancy Sign </h3>
            <img src="../images/180proj4/room.jpg" width="100px" alt="" />
            <br/>
        </article>
    </div>
</div>
</section>

### Jacobs Hall

<section id="two">
<div class="column">
    <div class="row">
        <article class="proj-item-2">
            <img src="../images/180proj4/jacobs1.jpg" width="200px" alt="" />
            <br/>
        </article>
        <article class="proj-item-2">
            <img src="../images/180proj4/jacobs2.jpg" width="200px" alt="" />
            <br/>
        </article>
    </div>
</div>
</section>

### McLaughlin Hall

<section id="two">
    <div class="column">
        <div class="row">
            <article class="proj-item-2">
                <img src="../images/180proj4/mclaugh1.jpg" width="200px" alt="" />
                <br/>
            </article>
            <article class="proj-item-2">
                <img src="../images/180proj4/mclaugh2.jpg" width="200px" alt="" />
                <br/>
            </article>
        </div>
    </div>
</section>

### Campanile Area

<section id="two">
    <div class="column">
        <div class="row">
            <article class="proj-item-2">
                <img src="../images/180proj4/campa1_resize.jpg" width="200px" alt="" />
                <br/>
            </article>
            <article class="proj-item-2">
                <img src="../images/180proj4/campa2_resize.jpg" width="200px" alt="" />
                <br/>
            </article>
        </div>
    </div>
</section>

<a name = "twoo"></a>
## 2. Recovering Homographies

In order to rectify an image or create a panorama, we must compute a homography using correspondence points in order to project an image to the same plane as another image. More specifically, we want to recover a 3x3 matrix such that:

<section id="two">
<div class="column">
    <div class="row">
        <article class="proj-item-1">
            <center><img src="../images/180proj4/fig1.png" width="30%" alt="" /></center>
            <br/>
        </article>
    </div>
</div>
</section>

where (x,y) corresponds to a point in our image to reproject, and (x',y') corresponds to the reference image whose plane we are aligning the other images to. 

To solve for this, we can expand the system out such that the right hand side only consists of our reference image points:

<section id="two">
<div class="column">
    <div class="row">
        <article class="proj-item-1">
            <center><img src="../images/180proj4/fig2.png" width="40%" alt="" /></center>
            <br/>
        </article>
    </div>
</div>
</section>

The above equation is only for one point (x,y). When using multiple correspondence points, I used numpy.vstack to stack the points vertically in the corresponding vectors and matrices. 

To solve for the homography matrix, I used least-squares, which would account for overdetermined systems. 

<a name = "three"></a>
## 3. Image Rectification

With the recovered homography matrix h, we can apply the matrix to the corners of an image as a projective transform. 

For image rectification, I computed a homography matrix between a rectangle within an image and a manually-defined set of coordinates. Then I transformed the rectangle coordinates within the image using h and divided by the scaling factor w (see the first equation above) to obtain the corresponding warped coordinates. After finding the bounding box of our resulting warp in this way, I used sk.draw.polygon and the inverse of the homography matrix to perform an inverse warp of all the coordinates within the bounding box and extract the corresponding pixel values from the original image. 

If the bounding box (after applying h and dividing by w) has negative coordinates, I computed and applied a translation matrix T so that the entire box had positive coordinates. I shifted the coordinates back using the inverse of T to extract the corresponding pixel values.

### Campanile Bells Diagram

<section id="two">
    <div class="column">
        <div class="row">
            <article class="proj-item-2">
                <img src="../images/180proj4/carillon_pts.png" width="200px" alt="" />
                <br/>
            </article>
            <article class="proj-item-2">
                <img src="../images/180proj4/carillon_rect.png" width="200px" alt="" />
                <br/>
            </article>
        </div>
    </div>
</section>

### Max Occupancy Sign

<section id="two">
    <div class="column">
        <div class="row">
            <article class="proj-item-2">
                <img src="../images/180proj4/sign_pts.png" width="200px" alt="" />
                <br/>
            </article>
            <article class="proj-item-2">
                <img src="../images/180proj4/sign_rect.png" width="200px" alt="" />
                <br/>
            </article>
        </div>
    </div>
</section>

<a name = "four"></a>
## 4. Image Blending

To create a panomara, I can use the same idea as image rectification, but instead of choosing 4 corners, I choose 8 correspondence points to recover my homography matrix. 

<section id="two">
<div class="column">
    <div class="row">
        <article class="proj-item-1">
            <center><img src="../images/180proj4/jacobs_corresponding_pts.png" width="700vw" alt="" /></center>
        </article>
    </div>
</div>
</section>

Creating a panorama requires computing a bigger bounding box that encompasses both the warped images and the reference image. To do this, I calculated the bounding boxes of all images after warping and translating (taking into account negative coordinates) and took the max x and y values out of the bounding box coordinates. After computing this bounding box, I created an empty array of the size of the bounding box and populated its elements using pixel values and coordinate indices obtained from the warp and translation tranformations I had saved from prior computations. 

<section id="two">
<div class="column">
    <div class="row">
        <article class="proj-item-2">
            <img src="../images/180proj4/jacobs1_warp.png" width="100px" alt="" />
            <br/>
        </article>
        <article class="proj-item-2">
            <img src="../images/180proj4/jacobs2_warp.png" width="100px" alt="" />
            <br/>
        </article>
    </div>
</div>
</section>

<section id="two">
<div class="column">
    <div class="row">
        <article class="proj-item-1">
            <center><img src="../images/180proj4/jacobs_warp_naive.png" width="700vw" alt="" /></center>
        </article>
    </div>
</div>
</section>

When naively overlaying the warped image onto the reference image, there is often a visible seam due to differences in lighting between the images and human error in selecting the correspondence points. To fix this, I implemented blending. I applied the distance transform between alpha channels and created a mask for the areas where the distance transform value was greater in the warped image. 

### Distance Transform on alpha channels of Jacobs images

<section id="two">
<div class="column">
    <div class="row">
        <article class="proj-item-2">
            <img src="../images/180proj4/jacobs1_transform1.png" width="100px" alt="" />
            <br/>
        </article>
        <article class="proj-item-2">
            <img src="../images/180proj4/jacobs2_transform1.png" width="100px" alt="" />
            <br/>
        </article>
    </div>
</div>
<br/>
</section>

### Resulting mask from the distance transforms

<section id="two">
<div class="column">
    <div class="row">
        <article class="proj-item-1">
            <center><img src="../images/180proj4/jacobs_blend_mask.png" width="300vw" alt="" /></center>
        </article>
    </div>
</div>
</section>

### Mosaics

<section id="two">
<div class="column">
    <div class="row">
        <article class="proj-item-2">
            <h3>Naive Mosaic of Jacobs</h3>
            <img src="../images/180proj4/jacobs_warp_naive.png" width="100px" alt="" />
            <br/>
        </article>
        <article class="proj-item-2">
            <h3>Blended Mosaic</h3>
            <img src="../images/180proj4/jacobs_warp_blend.png" width="100px" alt="" />
            <br/>
        </article>
    </div>
</div>
</section>

<section id="two">
<div class="column">
    <div class="row">
        <article class="proj-item-2">
            <h3>Naive Mosaic of McLaughlin</h3>
            <img src="../images/180proj4/mclaugh_warp_naive.png" width="100px" alt="" />
            <br/>
        </article>
        <article class="proj-item-2">
            <h3>Blended Mosaic</h3>
            <img src="../images/180proj4/mclaugh_warp_blend.png" width="100px" alt="" />
            <br/>
        </article>
    </div>
</div>
</section>

<section id="two">
<div class="column">
    <div class="row">
        <article class="proj-item-2">
            <h3>Naive Mosaic of SDH Area</h3>
            <img src="../images/180proj4/sdh_warp_naive.png" width="100px" alt="" />
            <br/>
        </article>
        <article class="proj-item-2">
            <h3>Blended Mosaic</h3>
            <img src="../images/180proj4/sdh_warp_blend.png" width="100px" alt="" />
            <br/>
        </article>
    </div>
</div>
</section>

<center> --- THE END --- </center>