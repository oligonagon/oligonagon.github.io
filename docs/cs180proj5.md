---
layout: project
title: Fun with Diffusion Models
permalink: /cs180proj5/
--- 

<h1 style="text-align:center">Fun with Diffusion Models</h1>
<h2 style="text-align:center">Fall 2024</h2>

<p align="center">In this project, I used the Deepfloyd diffusion model to explore different diffusion methods and noise levels. In the second part, I created my own UNet and diffusion model from scratch.
</p>

<center><h2> Table of Contents </h2>

1. <a href="#one">Sampling from the Model</a><br/>
2. <a href="#twoo">Implementing the Forward Process</a><br/>
3. <a href="#three">Classical Denoising</a><br/>
4. <a href="#four">Implementing Iterative Denoising</a><br/>
5. <a href="#five">Diffusion Model Sampling</a><br/>
6. <a href="#six">Classifier Free Guidance</a><br/>
7. <a href="#seven">Image-to-image Translation</a><br/>
8. <a href="#eight">Editing Hand-drawn and Web Images</a><br/>
9. <a href="#nine">Inpainting</a><br/>
10. <a href="#ten">Text-conditioned Image-to-image Translation</a><br/>
11. <a href="#eleven">Visual Anagrams</a><br/>
12. <a href="#twelve">Hybrid Images</a><br/>
<br/>

Diffusion Models from Scratch
<br/>
13. <a href="#thirteen">Training a Single-Step Denoising UNet</a><br/>
14. <a href="#fourteen">Training a Diffusion Model</a><br/>

</center>
<br/>
<a name = "one"></a>

## 1. Sampling from the Model

Below are the images I used for the first part of this project (sections 2-4).
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

<a name = "twoo"></a>

## 2. Implementing the Forward Process

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

<a name = "three"></a>
## 3. Classical Denoising

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

<a name = "four"></a>

## 4. Implementing Iterative Denoising

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

<a name = "five"></a>

## 5. Diffusion Model Sampling

To obtain candidate correspondence points, I use Harris corner detection to detect corners in the two images. Corners are good candidates for correspondence since they capture change in all directions, as opposed to an edge. Corners also worked well when manually defining correspondence points.

<section id="two">
<div class="column">
    <div class="row">
        <article class="proj-item-1">
            <center><img src="../images/180proj4b/bccs_harris_bad.png" width="700vw" alt="" /></center>
        </article>
    </div>
</div>
</section>

<a name = "six"></a>

## 6. Classifier Free Guidance

As one might notice above, Harris corner detection picks up too many candidate points... To help filter out candidate points, I implement Adaptive Non-Maximal Supression (ANMS). For each candidate point p, I keep the closest neighbor such that their strength (obtained from the Harris matrix) times the crobust (which I set to 0.9) is greater than the strength of p. I used a KDTree for efficient neighbor searching.

By implementing ANMS, we are able to keep a number of candidate points that are evenly distributed throughout the image AND representing relatively strong corners. We are also able to choose how many candidate points k we would like to keep, by picking the k top points ordered by distance.

<section id="two">
<div class="column">
    <h3>ANMS on Berkeley City Club, top 500 points</h3>
    <div class="row">
        <article class="proj-item-2">
            <img src="../images/180proj4b/bcc2_harris_anms.png" width="100px" alt="" />
            <br/>
        </article>
        <article class="proj-item-2">
            <img src="../images/180proj4b/bcc3_harris_anms.png" width="100px" alt="" />
            <br/>
        </article>
    </div>
</div>
</section>

<a name = "seven"></a>

## 7. Image-to-image Translation

Now that we have our candidate points after ANMS, we would like to further narrow them down by matching them across the two images. First, we extract feature descriptors from each point. This means that for a point p, we obtain the 40x40 pixel patch around that point, Gaussian blur the patch, and subsample to obtian a 8x8 patch.

Some examples of 8x8 patches, after normalization:
<section id="two">
<div class="column">
    <h3>Sample 8x8 patches of above left image</h3>
    <div class="row">
        <article class="proj-item-3">
            <img src="../images/180proj4b/bcc2_features0.png" width="200px" alt="" />
            <br/>
        </article>
        <article class="proj-item-3">
            <img src="../images/180proj4b/bcc2_features70.png" width="200px" alt="" />
            <br/>
        </article>
        <article class="proj-item-3">
            <img src="../images/180proj4b/bcc2_features100.png" width="200px" alt="" />
            <br/>
        </article>
    </div>
</div>
</section>

<a name = "eight"></a>

## 8. Editing Hand-drawn and Web Images

After obtaining 8x8 patches for all candidate points, I then match them with the 8x8 patches of candidate points on the other image using an SSD metric. For each candidate point p in the first image, I find the nearest neighbor and the second nearest neighbor. Two points are nearest neighbors if their 8x8 patches have the lowest SSD value out of all other pairings (with the first image's point). The second nearest neighbor is calcualted in a very similar way (using the second lowest SSD value).

Once the first nearest and second nearest neighbors are calculated for point p, I then calculate the ratio 1-nn/2-nn. Here, 1-nn and 2-nn correspond to the SSD values between p and the nearest/second nearest neighbors, respectively. If this ratio turns out to be below some threshold (which I set to 0.3 by default), then I decide to keep point p and its nearest neighbor. The idea is that if the 1-nn is a better match than the 2-nn by a large enough difference, then we have a real match between point p and the 1-nn neighbor. 

If the ratio does not meet the threshold, then we remove point p out of consideration. 

<section id="two">
<div class="column">
    <h3>Feature matching results of Berkeley City Club images, with 53 point pairs meeting a threshold under 0.3</h3>
    <div class="row">
        <article class="proj-item-1">
            <center><img src="../images/180proj4b/bccs_lowe_53.png" width="700vw" alt="" /></center>
        </article>
    </div>
</div>
</section>

<a name = "nine"></a>

## 9. Inpainting

On top of feature matching, we also use RANSAC to further filter out points that don't match well. The process is as follows:

<section id="two">
<div class="column">
    <div class="row">
        <article class="proj-item-2">
            <h3>Final mosaic, naive</h3>
            <img src="../images/180proj4b/bcc_auto_naive.png" width="100px" alt="" />
            <br/>
        </article>
        <article class="proj-item-2">
            <h3>Final mosaic, blended</h3>
            <img src="../images/180proj4b/bcc_auto.png" width="100px" alt="" />
            <br/>
        </article>
    </div>
</div>
</section>

<a name = "ten"></a>

## 10. Text-conditioned Image-to-image Translation

<section id="two">
<div class="column">
    <div class="row">
        <article class="proj-item-2">
            <h3>Final mosaic, naive</h3>
            <img src="../images/180proj4b/mural_auto_naive.png" width="100px" alt="" />
            <br/>
        </article>
        <article class="proj-item-2">
            <h3>Final mosaic, blended</h3>
            <img src="../images/180proj4b/mural_auto.png" width="100px" alt="" />
            <br/>
        </article>
    </div>
</div>
</section>
<br/>

<a name = "eleven"></a>

## 11. Visual Anagrams

<a name = "twelve"></a>

## 12. Hybrid Images

<center><h1>Diffusion Models from Scratch</h1></center>
In parts 1-4, I used manually defined correspondence points to compute a homography for the mosaic blending. But manually defining the points for each pair of images can be very tedious and also inaccurate. In the next parts, I implement a pipeline to autodetect suitable correspondence points. 

<a name = "thirteen"></a>

## 13. Training a Single-Step Denoising UNet

<a name = "fourteen"></a>

## 14. Training a Diffusion Model



## The coolest thing I have learned from this project

cool beans

<center> --- THE END --- </center>