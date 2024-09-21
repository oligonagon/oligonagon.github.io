---
layout: project
title: Fun with Filters and Frequencies
permalink: /cs180proj2/
--- 

<h1 style="text-align:center">Fun with Filters and Frequencies</h1>
<h2 style="text-align:center">Fall 2024</h2>

<p align="center">In this project, I demonstrate some interesting things you can do with filters and frequencies. 
</p>

## Table of Contents

1. <a href="#one">Fun with Filters</a>
    * <a href="#1a">Finite Difference Operator</a>
    * <a href="#1b">Derivative of Gaussian (DoG) Filter</a>
2. <a href="#twoo">Fun with Frequencies</a>
    * <a href="#2a">Image 'Sharpening'</a>
    * <a href="#2b">Hybrid Images</a>
    * <a href="#2c">Gaussian and Laplacian Stacks</a>
    * <a href="#2d">Multiresolution Blending</a>
        * <a href="#2di">Additional examples</a>

<a name = "one"></a>
<h2 align="center">
----- Fun with Filters ----- 
<br/>
</h2>

<a name = "1a"></a>
## Finite Difference Operator
In this part, I make 2 simple filters, Dx and Dy. By convolving an image with these filters, I can get the partial derivativeof the image in x and y.

<section id="two">
<div class="column">
    <div class="row">
        <article class="proj-item-1">
            <img src="../images/180proj2/cameraman.png" width="300vw" alt="" />
            <br/>
            <h3>cameraman.png</h3>
        </article>
    </div>
</div>
</section>

<section id="two">
<div class="column">
    <div class="row">
        <article class="proj-item-1">
            <img src="../images/180proj2/dxdy.png" width="700vw" alt="" />
        </article>
    </div>
</div>
</section>

Additionally, I can obtain the gradient magnitude of an image by taking the square root of the sum of squares between the images convolved by Dx and Dy (aka between the two images shown from above). Gradient magnitude then essentially gives us the magnitude of change at each location in the image. 

After taking the gradient magnitude, I binarize the image by determining a cutoff pixel value, then I setting all pixel values above that value to 1 and the remaining pixel values to 0. I tried to pick a cutoff value that keeps the cameraman's main outline. 

<section id="two">
<div class="column">
    <div class="row">
        <article class="proj-item-1">
            <img src="../images/180proj2/magbin.png" width="700vw" alt="" />
        </article>
    </div>
</div>
</section>

<a name = "1b"></a>
## Derivative of Gaussian (DoG) Filter
The results from directly using the Dx and Dy filters were a bit noisy. Here, I first blur the image with a Gaussian filter before applying Dx and Dy. 

<section id="two">
<div class="column">
    <div class="row">
        <article class="proj-item-1">
            <img src="../images/180proj2/gaussian_filter.png" width="200vw" alt="" />
        </article>
    </div>
</div>
</section>

<section id="two">
<div class="column">
    <div class="row">
        <article class="proj-item-1">
            <img src="../images/180proj2/blur_then_convolve_dxdy.png" width="700vw" alt="" />
        </article>
    </div>
</div>
</section>

<section id="two">
<div class="column">
    <div class="row">
        <article class="proj-item-1">
            <img src="../images/180proj2/blurred_magbin.png" width="700vw" alt="" />
        </article>
    </div>
</div>
</section>

Compared to the images from earlier, these images have much smoother edges. The smoother edges can be observed in all of the images (DxDy, gradient magnitude, and binarized). The lines are also thicker in the gradient magnitude and binarized images. 

It is possible to shuffle the steps to obtain the same results as above. We can convolve the Gaussian filter with Dx and Dy first, and then apply the convolved Gaussian filter to the image. Having one final convolution step can be advantageous in storage and computational efficiency.

<section id="two">
<div class="column">
    <div class="row">
        <article class="proj-item-1">
            <img src="../images/180proj2/gaussian_filter_convolved.png" width="400vw" alt="" />
        </article>
    </div>
</div>
</section>

<section id="two">
<div class="column">
    <div class="row">
        <article class="proj-item-1">
            <img src="../images/180proj2/convolve_then_blur_dxdy.png" width="700vw" alt="" />
        </article>
    </div>
</div>
</section>

<section id="two">
<div class="column">
    <div class="row">
        <article class="proj-item-1">
            <img src="../images/180proj2/blurred2_magbin.png" width="700vw" alt="" />
        </article>
    </div>
</div>
</section>

I'm not really sure why the first version (blurring then convolving) resulted in darked dxdy images, but the images between the two versions are relatively the same (it could be a display issue?). Additionally, the final binarized image between the two versions is exactly the same. 

<a name = "twoo"></a>
<h2 align="center">
----- Fun with Frequencies -----
<br/>
</h2>
<a name = "2a"></a>
## Image 'Sharpening'

In this part, I attempt to 'sharpen' various images by adding a bit more of the high frequencies into the image. To do this, I first apply a low pass (Gaussian) filter onto the image and the lowpass image from the original image to isolate the higher frequencies. Then I add these higher frequencies back into the original image, multiplied by some scaling coefficient alpha. There is some clipping of pixel values when this happens, but it does not seem to negatively affect the quality of the final result. 

I initially tried to implement the formal definition of an unsharp mask filter so that I could pre-compute one filter to use on the original image. For some reason this did not work out so well, so I stuck to the earlier (and I feel more intuitive) steps of blurring, subtracting, then scaling and readding the higher frequencies. 

<section id="two">
<div class="column">
    <div class="row">
        <article class="proj-item-1">
            <img src="../images/180proj2/taj_sharp.png" width="800vw" alt="" />
            <br/>
        </article>
    </div>
</div>
</section>

The results for taj.png look pretty good for alpha=2; you can see the bricks on the building more clearly. However, when alpha is further increased, some unwanted artifacts start to emerge. At alpha=10, some contrasting edges start to appear too thick, giving the image a bit of a fried look. 

<section id="two">
<div class="column">
    <div class="row">
        <article class="proj-item-1">
            <img src="../images/180proj2/yifang.png" width="800vw" alt="" />
            <br/>
        </article>
    </div>
</div>
</section>

You might need to open the image in a new tab and zoom in a bit to see this one. I really like the effect of 'sharpening' on this image as it makes the letters on the Yifang banner sharper, as well as the wall in the alleyway beside the shop. Similar to taj.png, you can see the introduction of some artifacts and graininess by alpha=10, and they are more obvious at alpha=20.

In addition to sharpening images, I try to 'recover' the sharpness of an image by blurring it and applying my sharpening function on the blurred image. I found this to somewhat work depending on the image and the level of blur. The below example hurts my eyes a bit...

<section id="two">
<div class="column">
    <div class="row">
        <article class="proj-item-1">
            <img src="../images/180proj2/noodles.png" width="800vw" alt="" />
            <br/>
        </article>
    </div>
</div>
<br/>
</section>

A more subtle yet successful example:

<section id="two">
<div class="column">
    <div class="row">
        <article class="proj-item-1">
            <img src="../images/180proj2/rose.png" width="800vw" alt="" />
            <br/>
        </article>
    </div>
</div>
</section>

<a name = "2b"></a>
## Hybrid Images

In this part, I try to create various hybrid images by taking advantage of our perception of frequencies at different distances. High frequencies dominate our perception, but at a distance, only the lower frequencies can be better seen.

To make a hybrid image, I align two images in a way that makes the illusion more effective. Then, I put one image through a lowpass filter and the other through a highpass filter. Finally, I add the two images together. For lowpass, I applied a Gaussian filter. For highpass, I subtracted the Gaussian blurred image from the original image.

<section id="two">
<div class="column">
    <div class="row">
        <article class="proj-item-2">
            <img src="../images/180proj2/catman_align.png" width="500vw" alt="" />
            <br/>
        </article>
        <article class="proj-item-2">
            <img src="../images/180proj2/catman_crop.png" height="200vw" alt="" />
            <br/>
        </article>
    </div>
</div>
<br/>
</section>

A hotdog:

<section id="two">
<div class="column">
    <div class="row">
        <article class="proj-item-3">
            <img src="../images/180proj2/hotdog.jpg" width="500vw" alt="" />
            <br/>
        </article>
        <article class="proj-item-3">
            <img src="../images/180proj2/dachshund.jpg" height="400vw" alt="" />
            <br/>
        </article>
        <article class="proj-item-3">
            <img src="../images/180proj2/hotdog.png" height="400vw" alt="" />
            <br/>
        </article>
    </div>
</div>
<br/>
</section>

In hopes of improving the hotdog example, I tested the effect of color on each of the images. However, because the dog is much darker relative to the hotdog, I feel that the color images made the hotdog even less visible... I think I would stick to the grayscale version for this example.

<section id="two">
<div class="column">
    <div class="row">
        <article class="proj-item-3">
            <img src="../images/180proj2/hotdog_grayfood.png" width="500vw" alt="" />
            <br/>
            <h3>color on lowpass image</h3>
        </article>
        <article class="proj-item-3">
            <img src="../images/180proj2/hotdog_graydog.png" height="400vw" alt="" />
            <br/>
            <h3>color on highpass image</h3>
        </article>
        <article class="proj-item-3">
            <img src="../images/180proj2/hotdog_color.png" height="400vw" alt="" />
            <br/>
            <h3>color on both images</h3>
        </article>
    </div>
</div>
<br/>
</section>

I really like the outcome of this one:

<section id="two">
<div class="column">
    <div class="row">
        <article class="proj-item-2">
            <img src="../images/180proj2/rock_aligned.png" width="500vw" alt="" />
            <br/>
        </article>
        <article class="proj-item-2">
            <img src="../images/180proj2/rockrock2.png" height="400vw" alt="" />
            <br/>
        </article>
    </div>
</div>
<br/>
</section>

Log magnitude of Fourier transform of the Rock and the rock:

<section id="two">
<div class="column">
    <div class="row">
        <article class="proj-item-2">
            <img src="../images/180proj2/rock_frequencies.png" width="500vw" alt="" />
            <br/>
        </article>
        <article class="proj-item-2">
            <img src="../images/180proj2/rockrock_frequencies.png" height="400vw" alt="" />
            <br/>
        </article>
    </div>
</div>
<br/>
</section>

Additionally, I tried adding colors to this example. There doesn't seem to be too much of a difference in terms of effectiveness, but I do like the addition of color to the lowpass image; it makes it more obvious that we are looking at a rock from a distance.

<section id="two">
<div class="column">
    <div class="row">
        <article class="proj-item-3">
            <img src="../images/180proj2/rockrock2_color.png" width="500vw" alt="" />
            <br/>
            <h3>color on lowpass image</h3>
        </article>
        <article class="proj-item-3">
            <img src="../images/180proj2/rockrock2_color2.png" height="400vw" alt="" />
            <br/>
            <h3>color on highpass image</h3>
        </article>
        <article class="proj-item-3">
            <img src="../images/180proj2/rockrock2_colorfull.png" height="400vw" alt="" />
            <br/>
            <h3>color on both images</h3>
        </article>
    </div>
</div>
<br/>
</section>

I also tried a high-frequency rock and a low-frequency Rock, but I don't think it turned out as well as the other way around. 

<section id="two">
<div class="column">
    <div class="row">
        <article class="proj-item-1">
            <img src="../images/180proj2/rockrock.png" width="300vw" alt="" />
            <br/>
        </article>
    </div>
</div>
<br/>
</section>

Finally, I tried combining a bird and a plane. This one did not turn out so well either. I think it's due to the large contrast between the bird and its background, in addition to the plane being bigger than the bird when overlapped.

<section id="two">
<div class="column">
    <div class="row">
        <article class="proj-item-2">
            <img src="../images/180proj2/birdplane_aligned.png" width="500vw" alt="" />
            <br/>
        </article>
        <article class="proj-item-2">
            <img src="../images/180proj2/birdplane.png" height="400vw" alt="" />
            <br/>
        </article>
    </div>
</div>
<br/>
</section>

<a name = "2c"></a>
## Gaussian and Laplacian Stacks

In this part I implement a Gaussian stack and a Laplacian stack for two images from the 1983 paper by Burt and Adelson. These stacks, as demonstrated in the next part, are useful when we want to blend two images together.

Note that unlike an image pyramid, the image does not get progressively smaller in a stack. For the Gaussian stack, I apply the same Gaussian filter 5 times, each time making the original image a bit blurrier. For the Laplacian stack, I take the difference between two neighboring images in the Gaussian stack and normalize the result. The Laplacian stack displays sub-band images, where at each level you get a the set of frequencies that is removed between Gaussian layers.

Gaussian and Laplacian stacks of the apple:
<section id="two">
<div class="column">
    <div class="row">
        <article class="proj-item-1">
            <img src="../images/180proj2/apple_blur.png" width="800vw" alt="" />
            <br/>
        </article>
    </div>
</div>
</section>
<section id="two">
<div class="column">
    <div class="row">
        <article class="proj-item-1">
            <img src="../images/180proj2/apple_laplaces.png" width="800vw" alt="" />
            <br/>
        </article>
    </div>
</div>
<br/>
</section>

Gaussian and Laplacian stacks of the orange:
<section id="two">
<div class="column">
    <div class="row">
        <article class="proj-item-1">
            <img src="../images/180proj2/orange_blur.png" width="800vw" alt="" />
            <br/>
        </article>
    </div>
</div>
</section>
<section id="two">
<div class="column">
    <div class="row">
        <article class="proj-item-1">
            <img src="../images/180proj2/orange_laplaces.png" width="800vw" alt="" />
            <br/>
        </article>
    </div>
</div>
<br/>
</section>

Something really cool about Laplacian stacks is that if you add all of the unnormalized levels up along with the blurriest Gaussian level, you can recover the original image.

<section id="two">
<div class="column">
    <div class="row">
        <article class="proj-item-2">
            <img src="../images/180proj2/apple_laplaces_sum.png" width="500vw" alt="" />
            <br/>
        </article>
        <article class="proj-item-2">
            <img src="../images/180proj2/orange_laplaces_sum.png" height="400vw" alt="" />
            <br/>
        </article>
    </div>
</div>
<br/>
</section>

<a name = "2d"></a>
## Multiresolution Blending

Using the Gaussian and Laplacian stacks, I can summon the oraple. 

I first created a mask, split vertically so that the left half was white and the right half was black. Then I created a Gaussian stack of the mask and multiplied each mask level elementwise by the corresponding Laplacian stack level of the apple and orange images (for the orange image, I multiplied by (1 - mask) to obtain the correct side of the orange). After normalizing each of these products, I took the sum across all of the levels, then added the blurriest Gaussian level multiplied by its corresponding mask level. After one final normalization, I obtained the beautiful oraple.

<section id="two">
<div class="column">
    <div class="row">
        <article class="proj-item-1">
            <img src="../images/180proj2/oraple.png" width="500vw" alt="" />
            <br/>
        </article>
    </div>
</div>
<br/>
</section>

A visual representation of the five Laplacian layers and the last Gaussian layer that was added to produce the final image. The Gaussian filter parameters I used were different from the ones I used to produce the Gaussian and Laplacian stacks from the previous part. This was because the same parameters did not produce a clear enough oraple. 

<section id="two">
<div class="column">
    <div class="row">
        <article class="proj-item-1">
            <img src="../images/180proj2/oraple_grid_final.jpg" width="400vw" alt="" />
            <br/>
        </article>
    </div>
</div>
<br/>
</section>

<a name = "2di"></a>
## More Multiresolution Blending

I used this multiresolution blending magic to create a dress that is both blue/black and white/gold.

<section id="two">
<div class="column">
    <div class="row">
        <article class="proj-item-1">
            <img src="../images/180proj2/the_dress_progression.png" width="700vw" alt="" />
            <br/>
        </article>
    </div>
</div>
<br/>
</section>

<section id="two">
<div class="column">
    <div class="row">
        <article class="proj-item-1">
            <img src="../images/180proj2/the_dress.png" width="300vw" alt="" />
            <br/>
        </article>
    </div>
</div>
<br/>
</section>

I also used this multiresolution blending magic and crafted an irregular mask to create a firewhale.

<section id="two">
<div class="column">
    <div class="row">
        <article class="proj-item-1">
            <img src="../images/180proj2/firewhale_progression.png" width="800vw" alt="" />
            <br/>
        </article>
    </div>
</div>
<br/>
</section>

<section id="two">
<div class="column">
    <div class="row">
        <article class="proj-item-1">
            <img src="../images/180proj2/firewhale.png" width="500vw" alt="" />
            <br/>
        </article>
    </div>
</div>
<br/>
</section>

Finally, I used this multiresolution blending magic on Aaron so that he can swim in Joffre Lakes.

<section id="two">
<div class="column">
    <div class="row">
        <article class="proj-item-3">
            <img src="../images/180proj2/aaron_og_crop.jpg" width="500vw" alt="" />
            <br/>
        </article>
        <article class="proj-item-3">
            <img src="../images/180proj2/joffre1.jpg" height="400vw" alt="" />
            <br/>
        </article>
        <article class="proj-item-3">
            <img src="../images/180proj2/joffre2_crop.jpg" height="400vw" alt="" />
            <br/>
        </article>
    </div>
</div>
</section>

<section id="two">
<div class="column">
    <div class="row">
        <article class="proj-item-2">
            <img src="../images/180proj2/aaron_swimming1.png" width="500vw" alt="" />
            <br/>
        </article>
        <article class="proj-item-2">
            <img src="../images/180proj2/aaron_swimming2_crop.png" height="400vw" alt="" />
            <br/>
        </article>
    </div>
    <br/>
</div>

<center>
<h2>--- THE END ---</h2>
</center>