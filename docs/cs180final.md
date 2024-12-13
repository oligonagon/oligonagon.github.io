---
layout: project
title: Final Project(s)
permalink: /cs180final/
--- 

<h1 style="text-align:center">Final Project(s)</h1>
<h2 style="text-align:center">Fall 2024</h2>

<center>
1. <a href="#one">Light Field Camera</a><br/>
2. <a href="#twoo">High Dynamic Range Imaging</a><br/>
</center>

<br/>
<a name = "one"></a>

## 1. Light Field Camera

In this project, I take data from a light field camera to simulate image focusing. I also simulate aperture adjustment to create different levels of depth of field.

### Image Refocusing

To focus certain layers of an image and blur other parts, I take the average of a grid of images taken at slightly different angles/positions. To change the focus, I shift each image based on their relative position from the reference image in the center of the image grid. After calculating this shift, I multiplied by a constant c. Adjusting c adjusts our layer of focus on the image. 

I implemented this on the chessboard image set. The set was a 17x17 image grid, where the image at [8,8] corresponded to our center image. After calculating the distance of each image from the center image, I multiplied each image by c. I found c values from [-0.2,0.7] to best capture the entire depth of the chessboard. lower c values focused on the farther parts of the board, while higher c values focused on the closer features.

<section id="two">
    <div class="column">
        <div class="row">
            <article class="proj-item-3">
                <img src="../images/180final/chessboard_4.png" width="200px" alt="" />
                <h3>c = 0</h3>
                <br/>
            </article>
            <article class="proj-item-3">
                <img src="../images/180final/chessboard_14.png" width="200px" alt="" />
                <h3>c = 0.5</h3>
                <br/>
            </article>
            <article class="proj-item-3">
                <img src="../images/180final/chessboard_18.png" width="200px" alt="" />
                <h3>c = 0.7</h3>
                <br/>
            </article>
        </div>
    </div>
</section>

<section id="two">
<div class="column">
    <div class="row">
        <article class="proj-item-1">
            <center>
                <h3>Chessboard Image Refocusing GIF</h3>
                <img src="../images/180final/chessboard_focus_gif.gif" width="70%" alt="" /></center>
            <br/>
        </article>
    </div>
</div>
</section>

### Aperture Adjustment

We can also simulate varying depths of field using light field data. While keeping c constant, we can adjust the number of images to average. For instance, a window size of [-2,2] would include the images surrounding the center image that are within 2 units of the grid. This would look like drawing a sub-square at the center within the image grid.

A smaller window corresponded with a smaller aperture and deeper DoF, while a larger window corresponded with a larger aperture and shallower DoF.

<section id="two">
    <div class="column">
        <div class="row">
            <article class="proj-item-3">
                <img src="../images/180final/chessboard_a_0.png" width="200px" alt="" />
                <h3>window = 0</h3>
                <br/>
            </article>
            <article class="proj-item-3">
                <img src="../images/180final/chessboard_a_2.png" width="200px" alt="" />
                <h3>window = [-4,4]</h3>
                <br/>
            </article>
            <article class="proj-item-3">
                <img src="../images/180final/chessboard_a_4.png" width="200px" alt="" />
                <h3>window = [-8,8]</h3>
                <br/>
            </article>
        </div>
    </div>
</section>

<section id="two">
<div class="column">
    <div class="row">
        <article class="proj-item-2">
            <img src="../images/180final/chessboard_aperture_back_gif.gif" width="100px" alt="" />
            <h3>Aperture Adjustment GIF, c=0</h3>
            <br/>
        </article>
        <article class="proj-item-2">
            <img src="../images/180final/chessboard_aperture_center_gif.gif" width="100px" alt="" />
            <h3>c=0.2</h3>
            <br/>
        </article>
    </div>
</div>
</section>

<a name = "twoo"></a>

## 2. High Dynamic Range Imaging

For this project, I attempt to adjust for differences in exposure levels in taking a picture. If we take a picture at multiple exposure levels and implement HDR, we can create a photo that appears more evenly lit. 

<section id="two">
<div class="column">
    <div class="row">
        <article class="proj-item-1">
            <center><img src="../images/180proj5/forwardnoise_eq.png" width="300vw" alt="" /></center>
        </article>
    </div>
</div>
<br/>
</section>

wee 

<section id="two">
    <div class="column">
        <div class="row">
            <article class="proj-item-3">
                <img src="../images/180proj5/campanile/test_image_t250.png" width="200px" alt="" />
                <h3>t = 250</h3>
                <br/>
            </article>
            <article class="proj-item-3">
                <img src="../images/180proj5/campanile/test_image_t500.png" width="200px" alt="" />
                <h3>t = 500</h3>
                <br/>
            </article>
            <article class="proj-item-3">
                <img src="../images/180proj5/campanile/test_image_t750.png" width="200px" alt="" />
                <h3>t = 750</h3>
                <br/>
            </article>
        </div>
    </div>
</section>

## The coolest thing I have learned from this project

weeee

<center> --- THE END --- </center>