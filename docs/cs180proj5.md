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
3. <a href="#three">Classical and One-Step Denoising</a><br/>
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

(explain)

Outputs (seed = 180)

<section id="two">
<div class="column">
    <div class="row">
        <article class="proj-item-1">
            <center>
                <h3>5 inference steps</h3>
                <img src="../images/180proj5/infsteps/stage2_5.png" width="70%" alt="" /></center>
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
                <h3>20 inference steps</h3>
                <img src="../images/180proj5/infsteps/stage2_20.png" width="70%" alt="" /></center>
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
                <h3>40 inference steps</h3>
                <img src="../images/180proj5/infsteps/stage2_40.png" width="70%" alt="" /></center>
            <br/>
        </article>
    </div>
</div>
</section>


<a name = "twoo"></a>

## 2. Implementing the Forward Process

(explain implementing forward function)
<section id="two">
    <div class="column">
        <div class="row">
            <article class="proj-item-3">
                <h3>t = 250</h3>
                <img src="../images/180proj5/campanile/test_image_t250.png" width="200px" alt="" />
                <br/>
            </article>
            <article class="proj-item-3">
                <h3>t = 500</h3>
                <img src="../images/180proj5/campanile/test_image_t500.png" width="200px" alt="" />
                <br/>
            </article>
            <article class="proj-item-3">
                <h3>t = 750</h3>
                <img src="../images/180proj5/campanile/test_image_t750.png" width="200px" alt="" />
                <br/>
            </article>
        </div>
    </div>
</section>

<a name = "three"></a>

## 3. Classical and One-Step Denoising

blah blah blah
(visualize side by side with reference image and noisy image?)

<center><h3>Denoise by Gaussian Blur</h3></center>

<section id="two">
    <div class="column">
        <div class="row">
            <article class="proj-item-3">
                <img src="../images/180proj5/campanile/clean_est_t250.png" width="200px" alt="" />
                <h3>t = 250</h3>
                <br/>
            </article>
            <article class="proj-item-3">
                <img src="../images/180proj5/campanile/clean_est_t500.png" width="200px" alt="" />
                <h3>t = 500</h3>
                <br/>
            </article>
            <article class="proj-item-3">
                <img src="../images/180proj5/campanile/clean_est_t750.png" width="200px" alt="" />
                <h3>t = 750</h3>
                <br/>
            </article>
        </div>
    </div>
</section>

<center><h3>One-step denoising by Pretrained Diffusion Model</h3></center>

<section id="two">
    <div class="column">
        <div class="row">
            <article class="proj-item-3">
                <img src="../images/180proj5/campanile/onestep_t250.png" width="200px" alt="" />
                <h3>t = 250</h3>
                <br/>
            </article>
            <article class="proj-item-3">
                <img src="../images/180proj5/campanile/onestep_t500.png" width="200px" alt="" />
                <h3>t = 500</h3>
                <br/>
            </article>
            <article class="proj-item-3">
                <img src="../images/180proj5/campanile/onestep_t750.png" width="200px" alt="" />
                <h3>t = 750</h3>
                <br/>
            </article>
        </div>
    </div>
</section>

<a name = "four"></a>

## 4. Implementing Iterative Denoising

weeee

<section id="two">
<div class="column">
    <div class="row">
        <article class="proj-item-1">
            <h3>Iteratively denoising the Campanile, starting at t=690 for the first image and decreasing t by 30 for each image:</h3>
            <center><img src="../images/180proj5/campa_iter_denoise.png" width="50%" alt="" /></center>
            <br/>
        </article>
    </div>
</div>
</section>

<section id="two">
<div class="column">
    <div class="row">
        <article class="proj-item-1">
            <center><h3>Final clean image:</h3>
            <img src="../images/180proj5/campanile/iterative_clean.png" width="50%" alt="" /></center>
            <br/>
        </article>
    </div>
</div>
</section>

### Comparison with results from Part 3:

make it all t=690!!

<section id="two">
    <div class="column">
        <div class="row">
            <article class="proj-item-3">
                <img src="../images/180proj5/campanile/clean_est_t750.png" width="200px" alt="" />
                <h3>Gaussian Blur Denoise, t=750</h3>
                <br/>
            </article>
            <article class="proj-item-3">
                <img src="../images/180proj5/campanile/onestep_t750.png" width="200px" alt="" />
                <h3>One-Step Denoise, t=750</h3>
                <br/>
            </article>
            <article class="proj-item-3">
                <img src="../images/180proj5/campanile/iterative_clean.png" width="200px" alt="" />
                <h3>Iterative Denoise, from t=690</h3>
                <br/>
            </article>
        </div>
    </div>
</section>

<a name = "five"></a>

## 5. Diffusion Model Sampling

weeee

<section id="two">
<div class="column">
    <div class="row">
        <article class="proj-item-1">
            <center><img src="../images/180proj5/sampling_gallery.png" width="50%" alt="" /></center>
        </article>
    </div>
</div>
</section>

<a name = "six"></a>

## 6. Classifier Free Guidance

pupusa 

<section id="two">
<div class="column">
    <div class="row">
        <article class="proj-item-1">
            <center><img src="../images/180proj5/noiselevels/sampling_cfg.png" width="100%" alt="" /></center>
        </article>
    </div>
</div>
</section>

<a name = "seven"></a>

## 7. Image-to-image Translation

hamburger

### Campanile:

<section id="two">
<div class="column">
    <div class="row">
        <article class="proj-item-1">
            <center><img src="../images/180proj5/ref/campanile.png" width="130%" alt="" /></center>
        </article>
    </div>
</div>
</section>

<section id="two">
<div class="column">
    <div class="row">
        <article class="proj-item-1">
            <center><img src="../images/180proj5/noiselevels/im2im_campanile.png" width="100%" alt="" /></center>
        </article>
        <br/>
    </div>
</div>
</section>
<br/>

### San Jose Circles:

<section id="two">
<div class="column">
    <div class="row">
        <article class="proj-item-1">
            <center><img src="../images/180proj5/ref/sj_im.png" width="130%" alt="" /></center>
        </article>
    </div>
</div>
</section>

<section id="two">
<div class="column">
    <div class="row">
        <article class="proj-item-1">
            <center><img src="../images/180proj5/noiselevels/im2im_sj.png" width="100%" alt="" /></center>
        </article>
        <br/>
    </div>
</div>
</section>
<br/>

### Bear:

<section id="two">
<div class="column">
    <div class="row">
        <article class="proj-item-1">
            <center><img src="../images/180proj5/ref/bear_im.png" width="130%" alt="" /></center>
        </article>
        <br/>
    </div>
</div>
</section>

<section id="two">
<div class="column">
    <div class="row">
        <article class="proj-item-1">
            <center><img src="../images/180proj5/noiselevels/im2im_bear.png" width="100%" alt="" /></center>
        </article>
        <br/>
    </div>
</div>
</section>
<br/>

### Yifang Pancakes:

<section id="two">
<div class="column">
    <div class="row">
        <article class="proj-item-1">
            <center><img src="../images/180proj5/ref/pancake_im.png" width="130%" alt="" /></center>
        </article>
    </div>
</div>
</section>

<section id="two">
<div class="column">
    <div class="row">
        <article class="proj-item-1">
            <center><img src="../images/180proj5/noiselevels/im2im_pancake.png" width="100%" alt="" /></center>
        </article>
    </div>
</div>
</section>

<a name = "eight"></a>

## 8. Editing Hand-drawn and Web Images

oogabuga

### Web Image:

<section id="two">
<div class="column">
    <div class="row">
        <article class="proj-item-1">
            <center><img src="../images/180proj5/ref/peepohappy.png" width="130%" alt="" /></center>
        </article>
    </div>
</div>
</section>

<section id="two">
<div class="column">
    <div class="row">
        <article class="proj-item-1">
            <center><img src="../images/180proj5/noiselevels/peepohappy3.png" width="100%" alt="" /></center>
        </article>
    </div>
</div>
</section>
<br/>

### Hand Drawn Image of Bird:

<section id="two">
<div class="column">
    <div class="row">
        <article class="proj-item-1">
            <center><img src="../images/180proj5/ref/bird.png" width="130%" alt="" /></center>
        </article>
    </div>
</div>
</section>

<section id="two">
<div class="column">
    <div class="row">
        <article class="proj-item-1">
            <center><img src="../images/180proj5/noiselevels/bird3.png" width="100%" alt="" /></center>
        </article>
    </div>
</div>
</section>
<br/>

### Hand Drawn Image of Smile:

<section id="two">
<div class="column">
    <div class="row">
        <article class="proj-item-1">
            <center><img src="../images/180proj5/ref/smile.png" width="130%" alt="" /></center>
        </article>
    </div>
</div>
</section>

<section id="two">
<div class="column">
    <div class="row">
        <article class="proj-item-1">
            <center><img src="../images/180proj5/noiselevels/smile2.png" width="100%" alt="" /></center>
        </article>
    </div>
</div>
</section>
<br/>

### Hand Drawn Image of Millenium Falcon (scanned):

<section id="two">
<div class="column">
    <div class="row">
        <article class="proj-item-1">
            <center><img src="../images/180proj5/ref/falcon.png" width="130%" alt="" /></center>
        </article>
    </div>
</div>
</section>

<section id="two">
<div class="column">
    <div class="row">
        <article class="proj-item-1">
            <center><img src="../images/180proj5/noiselevels/falcon2.png" width="100%" alt="" /></center>
        </article>
    </div>
</div>
</section>

<a name = "nine"></a>

## 9. Inpainting

ungabunga

<section id="two">
<div class="column">
    <center><h3>Inpainted Campanile</h3></center>
    <div class="row">
        <article class="proj-item-1">
            <center><img src="../images/180proj5/campa_inpaint.png" width="500vw" alt="" /></center>
        </article>
    </div>
</div>
</section>

<section id="two">
<div class="column">
    <center><h3>Inpainted Kiryu</h3></center>
    <div class="row">
        <article class="proj-item-1">
            <center><img src="../images/180proj5/kiryu_inpaint.png" width="500vw" alt="" /></center>
        </article>
    </div>
</div>
</section>

<section id="two">
<div class="column">
    <center><h3>Inpainted borb</h3></center>
    <div class="row">
        <article class="proj-item-1">
            <center><img src="../images/180proj5/borb_inpaint.png" width="500vw" alt="" /></center>
        </article>
    </div>
</div>
</section>

<section id="two">
<div class="column">
    <center><h3>Inpainted boba</h3></center>
    <div class="row">
        <article class="proj-item-1">
            <center><img src="../images/180proj5/boba_inpaint.png" width="500vw" alt="" /></center>
        </article>
    </div>
</div>
</section>

<a name = "ten"></a>

## 10. Text-conditioned Image-to-image Translation

eeeeeeee 

### Campanile, text conditioned with 'a rocket ship'

<section id="two">
<div class="column">
    <div class="row">
        <article class="proj-item-1">
            <center><img src="../images/180proj5/ref/campanile.png" width="130%" alt="" /></center>
        </article>
    </div>
</div>
</section>

<section id="two">
<div class="column">
    <div class="row">
        <article class="proj-item-1">
            <center><img src="../images/180proj5/noiselevels/inpaint_text_campanile.png" width="100%" alt="" /></center>
        </article>
    </div>
</div>
</section>
<br/>

### Kiryu, text conditioned with 'a photo of a hipster barista'

<section id="two">
<div class="column">
    <div class="row">
        <article class="proj-item-1">
            <center><img src="../images/180proj5/ref/kiryu.png" width="130%" alt="" /></center>
        </article>
    </div>
</div>
</section>

<section id="two">
<div class="column">
    <div class="row">
        <article class="proj-item-1">
            <center><img src="../images/180proj5/noiselevels/inpaint_text_kiryu.png" width="100%" alt="" /></center>
        </article>
    </div>
</div>
</section>
<br/>

### Kiryu in Yakuza 3, text conditioned with 'a photo of a hipster barista'
MAKE A BETTER ONE

<section id="two">
<div class="column">
    <div class="row">
        <article class="proj-item-1">
            <center><img src="../images/180proj5/ref/yakuza3.png" width="130%" alt="" /></center>
        </article>
    </div>
</div>
</section>

<section id="two">
<div class="column">
    <div class="row">
        <article class="proj-item-1">
            <center><img src="../images/180proj5/noiselevels/yakuza3.png" width="100%" alt="" /></center>
        </article>
    </div>
</div>
</section>
<br/>

<a name = "eleven"></a>

## 11. Visual Anagrams

oooo 

<section id="two">
<div class="column">
    <center><h3>Oil painting of a man vs oil painting of a campfire</h3></center>
    <div class="row">
        <article class="proj-item-1">
            <center><img src="../images/180proj5/flip_man_campfire.png" width="500vw" alt="" /></center>
        </article>
    </div>
</div>
</section>

<section id="two">
<div class="column">
    <center><h3>Cat vs lion</h3></center>
    <div class="row">
        <article class="proj-item-1">
            <center><img src="../images/180proj5/flip_cat_lion.png" width="500vw" alt="" /></center>
        </article>
    </div>
</div>
</section>

<section id="two">
<div class="column">
    <center><h3>Muffin vs chihuahua</h3></center>
    <div class="row">
        <article class="proj-item-1">
            <center><img src="../images/180proj5/flip_muffin_chihuahua.png" width="500vw" alt="" /></center>
        </article>
    </div>
</div>
</section>

<a name = "twelve"></a>

## 12. Hybrid Images

<section id="two">
<div class="column">
    <center><h3>Ominous waterfall</h3></center>
    <div class="row">
        <article class="proj-item-1">
            <center><img src="../images/180proj5/hybrids/waterfall_skull_hybrid.png" width="150vw" alt="" /></center>
        </article>
    </div>
</div>
</section>

<section id="two">
<div class="column">
    <center><h3>Cat or tiger?</h3></center>
    <div class="row">
        <article class="proj-item-1">
            <center><img src="../images/180proj5/hybrids/tiger_cat_hybrid.png" width="150vw" alt="" /></center>
        </article>
    </div>
</div>
</section>

<section id="two">
<div class="column">
    <center><h3>Dog or lion?</h3></center>
    <div class="row">
        <article class="proj-item-1">
            <center><img src="../images/180proj5/hybrids/dog_lion_hybrid.png" width="150vw" alt="" /></center>
        </article>
    </div>
</div>
</section>

<section id="two">
<div class="column">
    <center><h3>Dog or bear?</h3></center>
    <div class="row">
        <article class="proj-item-1">
            <center><img src="../images/180proj5/hybrids/dog_bear_hybrid.png" width="150vw" alt="" /></center>
        </article>
    </div>
</div>
</section>

<section id="two">
<div class="column">
    <center><h3>Campfire or old man?</h3></center>
    <div class="row">
        <article class="proj-item-1">
            <center><img src="../images/180proj5/hybrids/campfire_man_hybrid.png" width="150vw" alt="" /></center>
        </article>
    </div>
</div>
</section>

<a name = "thirteen"></a>

## 13. Training a Single-Step Denoising UNet

<a name = "fourteen"></a>

## 14. Training a Diffusion Model



## The coolest thing I have learned from this project

cool beans

<center> --- THE END --- </center>