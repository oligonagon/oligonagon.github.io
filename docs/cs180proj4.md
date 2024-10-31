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
4. <a href="#four">Image Blending</a><br/>
<br/>

Autostitching
<br/>
5. <a href="#five">Harris Corner Detection</a><br/>
6. <a href="#six">Adaptive Non-Maximal Suppression</a><br/>
7. <a href="#seven">Feature Descriptor Extraction</a><br/>
8. <a href="#eight">Feature Matching</a><br/>
9. <a href="#nine">RANSAC</a><br/>
10. <a href="#ten">Autostitching Results</a><br/></center>

<br/>
<a name = "one"></a>

## 1. Taking Pictures

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

### Sutardja Dai Hall Area

<section id="two">
    <div class="column">
        <div class="row">
            <article class="proj-item-2">
                <img src="../images/180proj4/sdh1_resize.jpg" width="200px" alt="" />
                <br/>
            </article>
            <article class="proj-item-2">
                <img src="../images/180proj4/sdh2_resize.jpg" width="200px" alt="" />
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

### Resulting mask from the distance transforms for blending Jacobs images

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
        </article>
    </div>
</div>
</section>
<br/>

<center><h1>Autostitching</h1></center>
In parts 1-4, I used manually defined correspondence points to compute a homography for the mosaic blending. But manually defining the points for each pair of images can be very tedious and also inaccurate. In the next parts, I implement a pipeline to autodetect suitable correspondence points. 

<a name = "five"></a>

## 5. Harris Corner Detection

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

## 6. Adaptive Non-Maximal Suppression

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

## 7. Feature Descriptor Extraction

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

<section id="two">
<div class="column">
    <h3>Sample 8x8 patches of above right image</h3>
    <div class="row">
        <article class="proj-item-3">
            <img src="../images/180proj4b/bcc3_features0.png" width="200px" alt="" />
            <br/>
        </article>
        <article class="proj-item-3">
            <img src="../images/180proj4b/bcc3_features70.png" width="200px" alt="" />
            <br/>
        </article>
        <article class="proj-item-3">
            <img src="../images/180proj4b/bcc3_features100.png" width="200px" alt="" />
            <br/>
        </article>
    </div>
</div>
</section>

<a name = "eight"></a>

## 8. Feature Matching

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

## 9. RANSAC

On top of feature matching, we also use RANSAC to further filter out points that don't match well. The process is as follows:

1. Sample 8 random pairs out of the feature matched pairs from part 8.
2. Compute a homography using the 8 random pairs.
3. For each point (out of the feature matched pairs) in the first image, apply homography and compute SSD between the result and the corresponding point in the second image.
4. If the SSD is below some predetermined threshold, then we keep this pair of points and add it to the set of inliers. 
5. Repeat steps 1-4 k times, and return the largest set of inliers. 

We use the largest set of inliers as our final correspondence points for computing a homography and warping images to build a mosaic. 

<section id="two">
<div class="column">
    <h3>Final correspondence points after 2000 iterations, with a threshold of under 30</h3>
    <div class="row">
        <article class="proj-item-1">
            <center><img src="../images/180proj4b/bcc_corresp_11.png" width="700vw" alt="" /></center>
        </article>
    </div>
</div>
</section>

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

## 10. Autostitching Results (of more images)
<br/>

## SF Mural
<section id="two">
<div class="column">
    <div class="row">
        <article class="proj-item-2">
            <img src="../images/180proj4b/mural1.jpg" width="100px" alt="" />
            <br/>
        </article>
        <article class="proj-item-2">
            <img src="../images/180proj4b/mural3.jpg" width="100px" alt="" />
            <br/>
        </article>
    </div>
</div>
</section>
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

## Paddington Station
<section id="two">
<div class="column">
    <div class="row">
        <article class="proj-item-2">
            <img src="../images/180proj4b/station1.jpg" width="100px" alt="" />
            <br/>
        </article>
        <article class="proj-item-2">
            <img src="../images/180proj4b/station3.jpg" width="100px" alt="" />
            <br/>
        </article>
    </div>
</div>
</section>
<section id="two">
<div class="column">
    <div class="row">
        <article class="proj-item-2">
            <h3>Final mosaic, naive</h3>
            <img src="../images/180proj4b/station_auto2_naive.png" width="100px" alt="" />
            <br/>
        </article>
        <article class="proj-item-2">
            <h3>Final mosaic, blended</h3>
            <img src="../images/180proj4b/station_auto2.png" width="100px" alt="" />
            <br/>
        </article>
    </div>
</div>
</section>
<br/>

## Lake
<section id="two">
<div class="column">
    <div class="row">
        <article class="proj-item-2">
            <img src="../images/180proj4b/lake1.jpg" width="100px" alt="" />
            <br/>
        </article>
        <article class="proj-item-2">
            <img src="../images/180proj4b/lake3.jpg" width="100px" alt="" />
            <br/>
        </article>
    </div>
</div>
</section>
<section id="two">
<div class="column">
    <div class="row">
        <article class="proj-item-2">
            <h3>Final mosaic, naive</h3>
            <img src="../images/180proj4b/lake_auto_naive.png" width="100px" alt="" />
            <br/>
        </article>
        <article class="proj-item-2">
            <h3>Final mosaic, blended</h3>
            <img src="../images/180proj4b/lake_auto.png" width="100px" alt="" />
            <br/>
        </article>
    </div>
</div>
</section>
<br/>

## The coolest thing I have learned from this project

Even though autostitching and RANSAC was fun to implement, I think the coolest part of the project was image rectification. Image rectification took me the longest to implement, and when it finally succeeded, I was very happy. Getting image rectification down also made image mosaicing later on less complicated.

<center> --- THE END --- </center>