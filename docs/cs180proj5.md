---
layout: project
title: Fun with Diffusion Models
permalink: /cs180proj5/
--- 

<h1 style="text-align:center">Fun with Diffusion Models</h1>
<h2 style="text-align:center">Fall 2024</h2>

<p align="center">In this project, I used the DeepFloyd diffusion model to explore different denoising approaches. In the second part, I created my own UNet and diffusion model from scratch.
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
15. <a href="#fifteen">Bell & Whistle: Denoising GIFs</a><br/>

</center>
<br/>
<a name = "one"></a>

## 1. Sampling from the Model

In this part, I create an instance of the DeepFloyd model (using seed=180) from HuggingFace and sample three images from them. These 3 images are sampled using text prompts 'an oil painting of a snowy mountain village', 'a man wearing a hat', and 'a rocket ship', respectively.

I also try sampling these images at different num_inference_steps. The results are shown below.

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

For num_inference_steps = 20 and 40, the quality of the image in relation to the text prompt seems to be quite well. I thought it was interesting how the model interprets 'a man wearing a hat' consistently as a photo of a man, instead of a painting or cartoon. In contrast, 'a rocket ship' is interpreted as a cartoon or drawing rather than a photo.

For num_inference_steps = 5, the images look quite grainy. 'an oil painting of a snowy mountain village' still produces a matching image, but the results for the other two prompts appear much more vague.

Additionally, I feel that num_inference_steps = 40 returns more detailed images than num_inference_steps = 20, such as more background detail (except for the man wearing a hat).

<a name = "twoo"></a>

## 2. Implementing the Forward Process

In this part, I implement the forward process of adding noise to a clean image. For a timestep t, I add an epsilon value that is randomly sampled from a standard normal distribution, scaled by a predetermined alpha value (determined by timestep). 
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

I perform the forward process to noise the Campanile at t=250, t=500, and t=750.
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

<a name = "three"></a>

## 3. Classical and One-Step Denoising

In this section, I attempt to denoise the Campanile images I generated above using classical and one-step methods.

First, I try adding a classic Gaussian blur...
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

We can see that this doesn't work out very well. Not only is the Campnile blurry, but we can still see the noisy pixels. 

Second, I try using a pretrained UNet diffusion model to denoise the images in one step. The diffusion model was trained with text conditioning, so for this part I use the prompt 'a high quality photo'. 

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

Looks better.. but the image still looks suboptimal when attempting to denoise from higher noise levels.

<a name = "four"></a>

## 4. Implementing Iterative Denoising

Diffusion models are designed to denoise iteratively rather than all in one step like attempted above. In this part. I iteratively denoise using a list of strided timesteps, starting at t=990 and decreasing t by 30 until reaching 0. This is done to save time (as denoising at every single time step would take too long).
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

Much better. The Campanile looks like a lighthouse! 

### Comparison with results from Part 3:

<section id="two">
    <div class="column">
        <div class="row">
            <article class="proj-item-3">
                <img src="../images/180proj5/campanile/clean_est_t690.png" width="200px" alt="" />
                <h3>Gaussian Blur Denoise, t=690</h3>
                <br/>
            </article>
            <article class="proj-item-3">
                <img src="../images/180proj5/campanile/onestep_t690.png" width="200px" alt="" />
                <h3>One-Step Denoise, t=690</h3>
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

Now that we have implemented iterative denoising, we can generate images from pure noise. Instead of starting at t=690 (or i_start=10), we can start at t=990 (i_start=0), which is an image made up of completely random noise.

10 examples with text prompt 'a high quality photo':
<section id="two">
<div class="column">
    <div class="row">
        <article class="proj-item-1">
            <center><img src="../images/180proj5/sampling_gallery.png" width="400vw" alt="" /></center>
        </article>
    </div>
</div>
</section>

<a name = "six"></a>

## 6. Classifier Free Guidance

It doesn't seem like the generated images above look very convincing or high-quality. In this section I implement Classifier-Free Guidance, where we compute both a conditional and unconditional noise estimate. We combine these two noise estimates using a scaling factor gamma that controls the CFG strength. When gamma=0, we get only the unconditional noise estimate. when gamma=1, we only get the conditional noise estimate.

When gamma > 1, we get much higher quality images.

5 examples with gamma = 7:
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

In this part, I add varying levels of noise to an image and denoise using iterative CFG. By denoising from slightly noisy images, we are allowing the diffusion model to be 'creative' and add things that were not there before. 

Below are images denoised at i_start = 1, 3, 5, 7, 10, and 20. The smaller the i_start (noisier image), the less similar the result is from the initial clean image.

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

What happens if we try the same image-to-image translation on non-realistic images and drawings? 

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
            <center><img src="../images/180proj5/noiselevels/peepohappy3.png" width="500vw" alt="" /></center>
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
            <center><img src="../images/180proj5/noiselevels/bird3.png" width="500vw" alt="" /></center>
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
            <center><img src="../images/180proj5/noiselevels/smile2.png" width="500vw" alt="" /></center>
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
            <center><img src="../images/180proj5/noiselevels/falcon2.png" width="500vw" alt="" /></center>
        </article>
    </div>
</div>
</section>

<a name = "nine"></a>

## 9. Inpainting

We can perform a masked denoising to only edit a specific part of an image. After each iterative step in denoising, we 'force' the parts outside of the mask to be the same image, while the parts inside the mask continue to be denoised. 

<section id="two">
<div class="column">
    <center><h3>Inpainted Campanile, masking the top of the tower</h3></center>
    <div class="row">
        <article class="proj-item-1">
            <center><img src="../images/180proj5/campa_inpaint.png" width="500vw" alt="" /></center>
        </article>
    </div>
</div>
</section>

<section id="two">
<div class="column">
    <center><h3>Inpainted Kiryu, masking the head</h3></center>
    <div class="row">
        <article class="proj-item-1">
            <center><img src="../images/180proj5/kiryu_inpaint.png" width="500vw" alt="" /></center>
        </article>
    </div>
</div>
</section>

<section id="two">
<div class="column">
    <center><h3>Inpainted borb, masking the head</h3></center>
    <div class="row">
        <article class="proj-item-1">
            <center><img src="../images/180proj5/borb_inpaint.png" width="500vw" alt="" /></center>
        </article>
    </div>
</div>
</section>

<a name = "ten"></a>

## 10. Text-conditioned Image-to-image Translation

The previous examples of image-to-image translation and inpainting were all used with the text prompt 'a high quality photo'. In this part, I change the text prompt to 'guide' the edits that the diffusion model makes (at varying noise levels like earlier).

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

### Kiryu in Yakuza 3, text conditioned with 'a photo of a hipster barista' to replace his head (the results don't seem to blend in the best way for every noise level)

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
            <center><img src="../images/180proj5/noiselevels/inpaint_text_yakuza3.png" width="100%" alt="" /></center>
        </article>
    </div>
</div>
</section>
<br/>

### Boba, text conditioned with 'a yifang fruit tea drink' to remove the TP Tea drink

<section id="two">
<div class="column">
    <div class="row">
        <article class="proj-item-1">
            <center><img src="../images/180proj5/ref/boba.jpg" width="100vw" alt="" /></center>
        </article>
    </div>
</div>
</section>

<section id="two">
<div class="column">
    <div class="row">
        <article class="proj-item-1">
            <center><img src="../images/180proj5/noiselevels/inpaint_text_boba.png" width="100%" alt="" /></center>
        </article>
    </div>
</div>
</section>
<br/>

<a name = "eleven"></a>

## 11. Visual Anagrams

In this part, I implement visual anagrams using diffusion models. To do this, I obtain two noise estimates. One is from a UNet denoising from a completely noisy image with one text prompt. The other is from a UNet denoising on the noisy image upside down, with a different text prompt. At each step of the iterative process, I compute these two noise estimates and take their average to obtain the final noise estimate. 

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

It is also possible to create hybrid images using diffusions models. Similar to the visual anagrams, I compute two separate noise estimates for two separate text prompts (but this time one of them is not denoising a flipped image). Then, I apply a lowpass filter on one estimate and a highpass filter on the other. Adding the two resulting values together gives me the final noise estimate. 

<section id="two">
<div class="column">
    <center><h3>Ominous waterfall</h3></center>
    <div class="row">
        <article class="proj-item-1">
            <center><img src="../images/180proj5/hybrid_waterfallskull.png" width="400vw" alt="" /></center>
        </article>
    </div>
</div>
</section>

<section id="two">
<div class="column">
    <center><h3>Cat or tiger?</h3></center>
    <div class="row">
        <article class="proj-item-1">
            <center><img src="../images/180proj5/hybrid_cattiger.png" width="400vw" alt="" /></center>
        </article>
    </div>
</div>
</section>

<section id="two">
<div class="column">
    <center><h3>Dog or lion?</h3></center>
    <div class="row">
        <article class="proj-item-1">
            <center><img src="../images/180proj5/hybrid_doglion.png" width="400vw" alt="" /></center>
        </article>
    </div>
</div>
</section>

<section id="two">
<div class="column">
    <center><h3>Dog or bear?</h3></center>
    <div class="row">
        <article class="proj-item-1">
            <center><img src="../images/180proj5/hybrid_dogbear.png" width="400vw" alt="" /></center>
        </article>
    </div>
</div>
</section>

<section id="two">
<div class="column">
    <center><h3>Campfire or old man?</h3></center>
    <div class="row">
        <article class="proj-item-1">
            <center><img src="../images/180proj5/hybrid_campfireman.png" width="400vw" alt="" /></center>
        </article>
    </div>
</div>
</section>

<a name = "thirteen"></a>

## 13. Training a Single-Step Denoising UNet

In these last two parts, I implement a UNet from scratch and use it to denoise MNIST digits. 

To add noise to a clean image x, we use the following equation, where epsilon is randomly sampled from a standard normal distribution.

<section id="two">
<div class="column">
    <div class="row">
        <article class="proj-item-1">
            <center><img src="../images/180proj5/5b/noising_eq.png" width="300vw" alt="" /></center>
        </article>
    </div>
</div>
</section>

We can vary the sigma value to adjust the strength of the noise.

Example of noising process using sigma = [0.0, 0.2, 0.4, 0.5, 0.6, 0.8, 1.0]. In the image, the sigma value increases from left to right:

<section id="two">
<div class="column">
    <div class="row">
        <article class="proj-item-1">
            <center><img src="../images/180proj5/5b/mnist_noisy.png" width="400vw" alt="" /></center>
        </article>
    </div>
</div>
</section>
<br/>

First, I implement an unconditional UNet.

<section id="two">
<div class="column">
    <div class="row">
        <article class="proj-item-1">
            <center>Unconditional UNet architecture</center>
            <center><img src="../images/180proj5/5b/unet_arch.png" width="600vw" alt="" /></center>
        </article>
    </div>
</div>
</section>

<section id="two">
<div class="column">
    <div class="row">
        <article class="proj-item-1">
            <center>UNet operations breakdown</center>
            <center><img src="../images/180proj5/5b/unet_ops.png" width="600vw" alt="" /></center>
        </article>
    </div>
</div>
</section>
<br/>

For the unconditioned UNet, we optimize over the following loss, where D is our denoiser and x is our clean image:

<section id="two">
<div class="column">
    <div class="row">
        <article class="proj-item-1">
            <center><img src="../images/180proj5/5b/unet_uncond_loss.png" width="300vw" alt="" /></center>
        </article>
    </div>
</div>
</section>

After training for 5 epochs with batch size = 256, sigma = 0.5, and hidden dimension D = 128...

<section id="two">
<div class="column">
    <div class="row">
        <article class="proj-item-1">
            <center>Training Loss curve plot</center>
            <center><img src="../images/180proj5/5b/uncond_loss.png" width="500vw" alt="" /></center>
        </article>
    </div>
</div>
</section>

For each of the images below, I display a row of the clean images, then a row of the images with sigma = 0.5 of noise added, and then a row of the denoised image

Sample results per epoch:

<section id="two">
<div class="column">
    <div class="row">
        <article class="proj-item-1">
            <center><img src="../images/180proj5/5b/uncond_epoch0.png" width="500vw" alt="" /></center>
        </article>
    </div>
</div>
</section>

<section id="two">
<div class="column">
    <div class="row">
        <article class="proj-item-1">
            <center><img src="../images/180proj5/5b/uncond_epoch1.png" width="500vw" alt="" /></center>
        </article>
    </div>
</div>
</section>

<section id="two">
<div class="column">
    <div class="row">
        <article class="proj-item-1">
            <center><img src="../images/180proj5/5b/uncond_epoch2.png" width="500vw" alt="" /></center>
        </article>
    </div>
</div>
</section>

<section id="two">
<div class="column">
    <div class="row">
        <article class="proj-item-1">
            <center><img src="../images/180proj5/5b/uncond_epoch3.png" width="500vw" alt="" /></center>
        </article>
    </div>
</div>
</section>

<section id="two">
<div class="column">
    <div class="row">
        <article class="proj-item-1">
            <center><img src="../images/180proj5/5b/uncond_epoch4.png" width="500vw" alt="" /></center>
        </article>
    </div>
</div>
</section>
<br/>

Sample results per noise level (varying sigma value). Sampled after training the UNet for 5 epochs:

<section id="two">
<div class="column">
    <div class="row">
        <article class="proj-item-1">
            <center><img src="../images/180proj5/5b/uncond_noise0.png" width="500vw" alt="" /></center>
        </article>
    </div>
</div>
</section>

<section id="two">
<div class="column">
    <div class="row">
        <article class="proj-item-1">
            <center><img src="../images/180proj5/5b/uncond_noise02.png" width="500vw" alt="" /></center>
        </article>
    </div>
</div>
</section>

<section id="two">
<div class="column">
    <div class="row">
        <article class="proj-item-1">
            <center><img src="../images/180proj5/5b/uncond_noise04.png" width="500vw" alt="" /></center>
        </article>
    </div>
</div>
</section>

<section id="two">
<div class="column">
    <div class="row">
        <article class="proj-item-1">
            <center><img src="../images/180proj5/5b/uncond_noise05.png" width="500vw" alt="" /></center>
        </article>
    </div>
</div>
</section>

<section id="two">
<div class="column">
    <div class="row">
        <article class="proj-item-1">
            <center><img src="../images/180proj5/5b/uncond_noise06.png" width="500vw" alt="" /></center>
        </article>
    </div>
</div>
</section>

<section id="two">
<div class="column">
    <div class="row">
        <article class="proj-item-1">
            <center><img src="../images/180proj5/5b/uncond_noise08.png" width="500vw" alt="" /></center>
        </article>
    </div>
</div>
</section>

<section id="two">
<div class="column">
    <div class="row">
        <article class="proj-item-1">
            <center><img src="../images/180proj5/5b/uncond_noise10.png" width="500vw" alt="" /></center>
        </article>
    </div>
</div>
</section>

<a name = "fourteen"></a>

## 14. Training a Diffusion Model

In this part, I train a UNet that can iteratively denoise an image. 

Hyperparameters used: 20 epochs, hidden dimension D = 64, batch size = 64.

Conditioned UNet architecture

<section id="two">
<div class="column">
    <div class="row">
        <article class="proj-item-1">
            <center><img src="../images/180proj5/5b/unet_cond_arch.png" width="600vw" alt="" /></center>
        </article>
    </div>
</div>
</section>

FCBlock operation breakdown

<section id="two">
<div class="column">
    <div class="row">
        <article class="proj-item-1">
            <center><img src="../images/180proj5/5b/unet_fcblock.png" width="600vw" alt="" /></center>
        </article>
    </div>
</div>
</section>

These conditioned UNet models predict the noise in an image, so the loss function is based on noise estimate epsilon instead of the image:

<section id="two">
<div class="column">
    <div class="row">
        <article class="proj-item-1">
            <center><img src="../images/180proj5/5b/noise_loss_eq.png" width="300vw" alt="" /></center>
        </article>
    </div>
</div>
</section>
<br/>

## Time-conditioned UNet

First, I train a version of the diffusion model that is conditioned on timestep t. This model predicts the noise in an image, given a noisy image and a timestep t. 

Training loss curve plot:

<section id="two">
<div class="column">
    <div class="row">
        <article class="proj-item-1">
            <center><img src="../images/180proj5/5b/cond_loss.png" width="500vw" alt="" /></center>
        </article>
    </div>
</div>
</section>
<br/>

Sampling results for epochs 1-5:

<section id="two">
<div class="column">
    <div class="row">
        <article class="proj-item-1">
            <center><img src="../images/180proj5/5b/time_epochs_a.png" width="500vw" alt="" /></center>
        </article>
    </div>
</div>
</section>
<br/>

Sampling results for epochs 16-20:
<section id="two">
<div class="column">
    <div class="row">
        <article class="proj-item-1">
            <center><img src="../images/180proj5/5b/time_epochs_b.png" width="500vw" alt="" /></center>
        </article>
    </div>
</div>
</section>
<br/>

## Class-conditional UNet

The time-conditioned UNet can iteratively denoise from a completely noisy image, but the resulting number is random. In this part, I implement a class-conditioned UNet that can selectively denoise digits decided by the user. To ensure that this UNet can still work without being conditioned on class, I implement a dropout rate of 10% (during training, 10% of the time the class conditioning vector gets set to 0). Additionally, I add CFG to with gamma = 5.0 to improve the quality of the results.

Training loss curve plot:

<section id="two">
<div class="column">
    <div class="row">
        <article class="proj-item-1">
            <center><img src="../images/180proj5/5b/class_loss.png" width="500vw" alt="" /></center>
        </article>
    </div>
</div>
</section>
<br/>

Sampling results for epochs 1:
<section id="two">
<div class="column">
    <div class="row">
        <article class="proj-item-1">
            <center><img src="../images/180proj5/5b/class_epoch0.png" width="500vw" alt="" /></center>
        </article>
    </div>
</div>
</section>
<br/>

Sampling results for epoch 5:
<section id="two">
<div class="column">
    <div class="row">
        <article class="proj-item-1">
            <center><img src="../images/180proj5/5b/class_epoch4.png" width="500vw" alt="" /></center>
        </article>
    </div>
</div>
</section>
<br/>

Sampling results for epoch 20:
<section id="two">
<div class="column">
    <div class="row">
        <article class="proj-item-1">
            <center><img src="../images/180proj5/5b/class_epoch19.png" width="500vw" alt="" /></center>
        </article>
    </div>
</div>
</section>
<br/>

<a name = "fifteen"></a>

## Denoising GIFs

Time-conditioned UNet:

<section id="two">
<div class="column">
    <div class="row">
        <article class="proj-item-1">
            <center><img src="../images/180proj5/5b/time.gif" width="500vw" alt="" /></center>
        </article>
    </div>
</div>
</section>
<br/>

Class-conditioned UNet:

<section id="two">
<div class="column">
    <div class="row">
        <article class="proj-item-1">
            <center><img src="../images/180proj5/5b/class.gif" width="500vw" alt="" /></center>
        </article>
    </div>
</div>
</section>
<br/>

Class-conditioned UNet with c vector of all zeroes:

<section id="two">
<div class="column">
    <div class="row">
        <article class="proj-item-1">
            <center><img src="../images/180proj5/5b/class_uncond.gif" width="500vw" alt="" /></center>
        </article>
    </div>
</div>
</section>
<br/>

## The coolest thing I have learned from this project

I think the coolest thing I learned from this project was how to implement iterative CFG denoising. It was very interesting to feed my own images into the diffusion model at different noise levels to see what interesting edits it could come up with. I think it's also impressive how for inpainting, the edits tend to be seamless with the rest of the image.

<center> --- THE END --- </center>