---
# Feel free to add content and custom Front Matter to this file.
# To modify the layout, see https://jekyllrb.com/docs/themes/#overriding-theme-defaults

layout: default
title: Home
permalink: /
---

#  CS 180: Project 1, Steven Luo

# Overview
[Sergei Mikhailovich Prokudin-Gorskii](https://en.wikipedia.org/wiki/Sergey_Prokudin-Gorsky) was a Russian chemist and photographer known for his pioneering work in color photography and his efforts to document early 20th-century Russia. He recorded three exposures of each scene onto a glass plate using a red, green, and blue filter, and envisioned special projectors would be able to combine the exposure and display a color image. Though he left Russia in 1918, these glass plate were purchased by the Library of Congress and [recently digitized](https://www.loc.gov/collections/prokudin-gorskii/about-this-collection/). 

Our goal is to turn a stitched negative into a color image. A stitched negative consists of the three color exposures stacked in one image file.

<p align="center">
    <img src="./tobolsk.jpg" alt="tobolsk" width="200"/>
    <img src="./monastery.jpg" alt="monastery" width="200"/>
    <img src="./cathedral.jpg" alt="cathedral" width="200"/>
    <p style="text-align: center;"><i>Examples of digitized glass plates: tobolsk, monastery, and cathedral.</i></p>
</p>

We want to use image processing techniques to align these plates to produce a color image with minimal artifacts. I used an alignment algorithm with Euclidean distance scoring and implemented an image pyramid to generate an initial set of color images, then further refined the output by using edges as features, adding white balancing, and automatically cropping images to remove undesired borders resulting from stacking the glass plates.

# Approach
We start by cropping the image into thirds to get the red, green, and blue plate, and stack them on top of each other to get a color image. A quick experiment shows that this image has many visual artifacts that distort the subject of the image.

<p align="center">
    <img src="./naive_stack_cathedral.jpg" alt="cathedral" width="400"/>
    <p style="text-align: center;"><i>Three plates stacked without alignment.</i></p>
</p>

I implemented an alignment function that finds the best displacement on each axis, then stacks the shifted plates. By finding the displacement that results in the best alignment when aligning the green to the blue then the red to the blue plate, we can stack the displaced green, displaced red, and blue plates together to get an improved color image. To evaluate the quality of an alignment, I use a scoring method that calculates the Euclidean distance between two images for every possible pair of (x,y) displacements within a specified search range; the returned, best-scoring image from my `align` function is the result of an x and y displacement from the search range that minimizes the Euclidean distance. I also implemented normalized cross-correlation as a scoring method, but it resulted in worse alignment and worse runtime.

To prevent the borders of the image from skewing the score computation, I only scored images based on the center 90% (removing 5% of image size from each side), slightly improving alignments. I considered preprocessing images by directly cropping the initial plates, but I decided not to in order to preserve the full image to allow for smarter cropping in the future.

In order to find the best alignment, I began with an exhaustive search of every possible pair of x and y displacements from a fixed range, for example [-20, 20]. Not only is exhaustive search slow (there are (range width)<sup>2</sup> combinations — in this case, 41<sup>2</sup> combinations to check), it also risks missing the optimal displacement pair if the optimum falls outside of the search range, especially likely on a higher resolution image. 

In order to speed up the alignment process, I implemented an image pyramid that recursively searches a small range of pixel shifts starting from a low resolution to the highest resolution. The smaller search range and the improved search method was vital in order to align the larger, higher-resolution `.tif` images, reducing the per-image runtime from tens of minutes to tens of seconds. This also improves the search heuristic: by centering the search range around the optimal displacement pair for a lower- (half) resolution image, we can shrink the search range significantly while still finding or ending up near the true optimal displacement pair, a speed up of several orders of magnitude.

As I improved my alignment and scoring methods, I noticed several images were consistently tricky to align: `melon`, `self-portrait`, `church`, and especially `emir`. These images are difficult for reasons such as their complex composition and inconsistent brightness between plates. The order of plate alignment also might have played a role, especially in `emir` which would probably have benefited from aligning to a different plate, such as red, instead of the blue, since there was not much red on the red plate. However, implementing the image pyramid and later bells and whistles successfully minimized artifacts on all of these images.

# Results

Displacement: G (x_g, y_g); R (x_r, y_r) indicates an x_g pixel shift on the x-axis and y_g pixel shift on the y-axis on the green plate; x_r shift on the x-axis and y_r shift on the y-axis on the red plate.

<table>
  <thead>
    <tr>
      <th colspan="2" style="text-align: center;">Low Quality Images (.jpg)</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align: center;"><img width="1000" alt="cathedral" src="./rgb_base_results/cathedral_euclidian_rgb_gx 2_gy 5_rx 3_ry 12_ time 0.13.jpg"> Cathedral <br /> Displacement: G (2, 5); R (3, 12) <br /> Runtime: 0.13 seconds</td>
      <td style="text-align: center;"><img width="1000" alt="monastery" src="./rgb_base_results/monastery_euclidian_rgb_gx 2_gy -3_rx 2_ry 3_ time 0.11.jpg"> Monastery <br /> Displacement: G (2, -3); R (2, 3) <br /> Runtime: 0.11 seconds</td>
    </tr>
    <tr>
      <td style="text-align: center;"><img width="1000" alt="tobolsk" src="./rgb_base_results/tobolsk_euclidian_rgb_gx 2_gy 3_rx 3_ry 6_ time 0.12.jpg"> Tobolsk <br /> Displacement: G (2, 3); R (3, 6) <br /> Runtime: 0.12 seconds</td>
      <td style="text-align: center;"></td>
    </tr>
  </tbody>
</table>

<table>
  <thead>
    <tr>
      <th colspan="2" style="text-align: center;">Full Quality Images (.tif)</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align: center;"><img width="1000" alt="church" src="./rgb_base_results/church_euclidian_rgb_gx 3_gy 25_rx -5_ry 58_ time 10.94.jpg">Church <br /> Displacement: G (3, 25); R (-5, 58) <br /> Runtime: 10.94 seconds</td>
      <td style="text-align: center;"><img width="1000" alt="emir" src="./rgb_base_results/emir_euclidian_rgb_gx 24_gy 49_rx 43_ry 88_ time 10.41.jpg">Emir  <br /> Displacement: G (24, 49); R (43, 88) <br /> Runtime: 10.41 seconds</td>
    </tr>
    <tr>
      <td style="text-align: center;"><img width="1000" alt="harvesters" src="./rgb_base_results/harvesters_euclidian_rgb_gx 16_gy 60_rx 13_ry 124_ time 10.38.jpg">Harvesters <br /> Displacement: G (16, 60); R (13, 24) <br /> Runtime: 10.38 seconds</td>
      <td style="text-align: center;"><img width="1000" alt="icon" src="./rgb_base_results/icon_euclidian_rgb_gx 17_gy 41_rx 23_ry 89_ time 11.21.jpg">Icon <br /> Displacement: G (17, 41); R (23, 89) <br /> Runtime: 11.21 seconds</td>
    </tr>
    <tr>
      <td style="text-align: center;"><img width="1000" alt="lady" src="./rgb_base_results/lady_euclidian_rgb_gx 8_gy 55_rx 12_ry 111_ time 10.49.jpg">Lady <br /> Displacement: G (8, 55); R (12, 111) <br /> Runtime: 10.49 seconds</td>
      <td style="text-align: center;"><img width="1000" alt="melons" src="./rgb_base_results/melons_euclidian_rgb_gx 9_gy 82_rx 11_ry 177_ time 11.0.jpg">Melons <br /> Displacement: G (9, 82); R (11, 177) <br /> Runtime: 11.00 seconds</td>
    </tr>
    <tr>
      <td style="text-align: center;"><img width="1000" alt="onion" src="./rgb_base_results/onion_church_euclidian_rgb_gx 26_gy 51_rx 36_ry 108_ time 11.31.jpg">Onion-Shaped Church <br /> Displacement: G (26, 51); R (36, 108) <br /> Runtime: 11.31 seconds</td>
      <td style="text-align: center;"><img width="1000" alt="sculpture" src="./rgb_base_results/sculpture_euclidian_rgb_gx -11_gy 33_rx -27_ry 140_ time 11.02.jpg">Sculpture <br /> Displacement: G (-11, 33); R (-27, 140) <br /> Runtime: 11.02 seconds</td>
    </tr>
    <tr>
      <td style="text-align: center;"><img width="1000" alt="self portrait" src="./rgb_base_results/self_portrait_euclidian_rgb_gx 29_gy 79_rx 34_ry 175_ time 11.83.jpg">Self-Portrait <br /> Displacement: G (29, 79); R (34, 175) <br /> Runtime: 11.83 seconds</td>
      <td style="text-align: center;"><img width="1000" alt="three gen" src="./rgb_base_results/three_generations_euclidian_rgb_gx 13_gy 55_rx 10_ry 112_ time 11.52.jpg">Three Generations <br /> Displacement: G (13, 55); R (10, 112) <br /> Runtime: 11.52 seconds</td>
    </tr>
    <tr>
      <td style="text-align: center;"><img width="1000" alt="train" src="./rgb_base_results/train_euclidian_rgb_gx 6_gy 42_rx 32_ry 87_ time 11.8.jpg">Train <br /> Displacement: G (6, 42); R (32, 87) <br /> Runtime: 11.8 seconds</td>
      <td style="text-align: center;"></td>
    </tr>
    <!-- <tr>
      <td style="text-align: center;"><img width="1000" alt="" src="">Displacement: G (, ); R (, ) <br /> Runtime:  seconds</td>
      <td style="text-align: center;"><img width="1000" alt="" src="">Displacement: G (, ); R (, ) <br /> Runtime:  seconds</td>
    </tr> -->
  </tbody>
</table>


<!-- | | | 
|:-------------------------:|:-------------------------:|
| <img width="1000" alt="" src=""> | <img width="1000" alt="" src=""> |
| <img width="1000" alt="" src=""> | <img width="1000" alt="" src=""> |
| <img width="1000" alt="" src=""> | <img width="1000" alt="" src=""> |
| <img width="1000" alt="" src=""> | <img width="1000" alt="" src=""> |
| <img width="1000" alt="" src=""> | <img width="1000" alt="" src=""> |
| | |  -->

# Bells and Whistles

## Better Features: Edge Detection with the Sobel Operator

Instead of aligning colors, what if we aligned shapes? It turns out the [Sobel operator](https://en.wikipedia.org/wiki/Sobel_operator) allows us to do just that: it produces an "edge map" (black and white image emphasizing edges) from an image, and by aligning the edge maps we can better align our images based on the subjects in them. Several examples are provided below!

The Sobel operator uses the convolution of two special 3x3 kernels with the input image to approximate the derivatives in the vertical and horizontal directions. This is a good approximation edges because it reveals sudden changes — like when one object ends and another begins — in pixel intensities in two directions. A large-magnitude derivative would indicate a large shift in pixel intensities, which could be indicative of an edge between objects. However, because pixel values are discrete, we cannot simply calculate a first derivative and instead need to use convolutions to estimate them, as we do with the Sobel operator to estimate the gradient of intensity values at a pixel. We interpret pixels that have gradients of large magnitude as likely to be part of an edge in the image.

Here's what we get when we apply a Sobel filter to each plate of `onion_church.tif`. We can see that aligning the edges will obviously result in a precisely aligned image.

<p align="center">
    <img src="./r_onion.jpg" alt="r onion" width="200"/>
    <img src="./g_onion.jpg" alt="b onion" width="200"/>
    <img src="./b_onion.jpg" alt="g onion" width="200"/>
    <p style="text-align: center;"><i>Edges of each glass plate found with a Sobel filter.</i></p>
</p>

This example shows the clear improvement in `emir.tif` when aligning based on RGB similarity to aligning based on edges:

<p align="center">
    <img src="./r_emir.jpg" alt="r emir" width="200"/>
    <img src="./g_emir.jpg" alt="b emir" width="200"/>
    <img src="./b_emir.jpg" alt="g emir" width="200"/>
    <p style="text-align: center;"><i>Edges of each glass plate found with a Sobel filter.</i></p>
</p>

| RGB | Edge | 
|:-------------------------:|:-------------------------:|
|<img width="1000" alt="emir rgb" src="./rgb_base_results/emir_euclidian_rgb_gx 24_gy 49_rx 43_ry 88_ time 10.41.jpg"> Displacement: G (24, 49); R (43, 88) <br /> Runtime: 10.41 seconds | <img width="1000" alt="emir edges" src="./sobel_base_results/emir_euclidian_sobel_gx 23_gy 49_rx 40_ry 107_ time 11.36.jpg"> Displacement: G (23, 49); R (40, 107) <br /> Runtime: 11.36 seconds|

Aligning via edges works great when the object is "busy" with edges in all directions to align, but performance is not as strong on images that are "flat", with fewer edges mostly going in the same direction. Intuitively, alignment is harder with less reference points. Notice how in the following images, after cropping to the center 90% we really only have one horizontal edge to align the three images with. This helps with alignment along the y-axis, but doesn't give us much to work with on the x-axis (you may need to zoom in to see the lines).

<br/>
<p align="center">
    <img src="./r_sunset.jpg" alt="r sunset" width="200"/>
    <img src="./g_sunset.jpg" alt="b sunset" width="200"/>
    <img src="./b_sunset.jpg" alt="g sunset" width="200"/>
    <p style="text-align: center;"><i>Edges of each glass plate found with a Sobel filter.</i></p>
</p>

<br/>
<p align="center">
    <img width="500" alt="sunset" src="./exedge_results/sunset_euclidian_sobel_gx 2_gy 52_rx -35_ry 119_ time 11.28.jpg">
</p>


On the final image, look at the sun on the horizon and notice how the red plate is slightly misaligned along the x-axis. Note that all "original" images below are all generated using the Sobel operator to align glass plates.

## Just For Fun
I was curious about what other photos the Library of Congress had in their collection (and if my code would work on these images) so I downloaded 15 more images from the catalog that caught my eye, and that seemed hard to align. In my comparisons using this set of images, I found my above findings held on this new set of images.

For example, using edges as a feature significantly improved alignment on tricky images, especially when the brightness on the three color channel images were not consistent. This example shows the clear improvement in `yurt.tif` when aligning based on RGB similarity to aligning based on edges:

| RGB | Edge | 
|:-------------------------:|:-------------------------:|
|<img width="1000" alt="yurt rgb" src="./exrgb_results/yurt_euclidian_rgb_gx 38_gy 48_rx -9_ry 115_ time 10.57.jpg"> Displacement: G (38, 48); R (-9, 115) <br /> Runtime: 10.57 seconds | <img width="1000" alt="yurt edges" src="./exedge_results/yurt_euclidian_sobel_gx 37_gy 53_rx 56_ry 107_ time 12.5.jpg"> Displacement: G (37, 57); R (56, 107) <br /> Runtime: 12.5 seconds|

Edge-aligned results are found in the following table:

<table>
  <thead>
    <tr>
      <th colspan="2" style="text-align: center;">Full Quality Images (.tif)</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align: center;"><img width="1000" alt="workshop" src="./exedge_results/workshop_euclidian_sobel_gx 15_gy 73_rx 14_ry 154_ time 13.3.jpg">Workshop <br /> Displacement: G (15, 73); R (14, 154) <br /> Runtime: 13.3 seconds</td>
      <td style="text-align: center;"><img width="1000" alt="greenhouse" src="./exedge_results/greenhouse_euclidian_sobel_gx 28_gy 59_rx 34_ry 126_ time 12.6.jpg">Greenhouse <br /> Displacement: G (28, 59); R (24, 126) <br /> Runtime: 12.6 seconds</td>
    </tr>
    <tr>
      <td style="text-align: center;"><img width="1000" alt="ditches" src="./exedge_results/ditches_euclidian_sobel_gx -27_gy 33_rx -55_ry 74_ time 10.85.jpg">Ditches <br /> Displacement: G (-27, 33); R (-55, 74) <br /> Runtime: 10.85 seconds</td>
      <td style="text-align: center;"><img width="1000" alt="vases" src="./exedge_results/vases_euclidian_sobel_gx 5_gy 25_rx 9_ry 110_ time 11.69.jpg">Vases <br /> Displacement: G (5, 25); R (9, 110) <br /> Runtime: 11.69 seconds</td>
    </tr>
    <tr>
      <td style="text-align: center;"><img width="1000" alt="forest" src="./exedge_results/forest_euclidian_sobel_gx -41_gy 75_rx -69_ry 114_ time 11.64.jpg">Forest <br /> Displacement: G (-41, 75); R (-69, 114) <br /> Runtime: 11.64 seconds</td>
      <td style="text-align: center;"><img width="1000" alt="sunset" src="./exedge_results/sunset_euclidian_sobel_gx 2_gy 52_rx -35_ry 119_ time 11.28.jpg">Sunset <br /> Displacement: G (2, 52); R (-35, 119) <br /> Runtime: 11.28 seconds</td>
    </tr>
    <tr>
      <td style="text-align: center;"><img width="1000" alt="lugano" src="./exedge_results/lugano_euclidian_sobel_gx -17_gy 41_rx -29_ry 91_ time 12.35.jpg">Lugano <br /> Displacement: G (-17, 41); R (-29, 91) <br /> Runtime: 12.35 seconds</td>
      <td style="text-align: center;"><img width="1000" alt="poppies" src="./exedge_results/poppies_euclidian_sobel_gx -1_gy 12_rx -3_ry 98_ time 11.15.jpg">Poppies <br /> Displacement: G (-1, 12); R (-3, 98) <br /> Runtime: 11.15 seconds</td>
    </tr>
    <tr>
      <td style="text-align: center;"><img width="1000" alt="carpet" src="./exedge_results/carpet_euclidian_sobel_gx -16_gy 48_rx -52_ry 91_ time 12.12.jpg">Carpet <br /> Displacement: G (-16, 48); R (-52, 91) <br /> Runtime: 12.12 seconds</td>
      <td style="text-align: center;"><img width="1000" alt="italy" src="./exedge_results/italy_euclidian_sobel_gx 22_gy 38_rx 36_ry 77_ time 12.1.jpg">Italy <br /> Displacement: G (22, 38); R (36, 77) <br /> Runtime: 12.1 seconds</td>
    </tr>
    <tr>
      <td style="text-align: center;"><img width="1000" alt="lilacs" src="./exedge_results/lilacs_euclidian_sobel_gx -11_gy 43_rx -36_ry 95_ time 12.35.jpg">Lilacs <br /> Displacement: G (-11, 43); R (-36, 95) <br /> Runtime: 12.35 seconds</td>
      <td style="text-align: center;"><img width="1000" alt="peonies" src="./exedge_results/peony_euclidian_sobel_gx 20_gy 76_rx 31_ry 155_ time 13.42.jpg">Peony <br /> Displacement: G (20, 76); R (31, 155) <br /> Runtime: 13.42 seconds</td>
    </tr>
    <tr>
      <td style="text-align: center;"><img width="1000" alt="hill" src="./exedge_results/hill_euclidian_sobel_gx 5_gy 39_rx 4_ry 130_ time 12.03.jpg">Hill <br /> Displacement: G (5, 39); R (4, 130) <br /> Runtime: 12.03 seconds</td>
      <td style="text-align: center;"><img width="1000" alt="barracks" src="./exedge_results/barracks_euclidian_sobel_gx 1_gy 11_rx 0_ry 73_ time 11.22.jpg">Barracks <br /> Displacement: G (1, 11); R (0, 73) <br /> Runtime: 11.22 seconds</td>
    </tr>
  </tbody>
</table>

## White Balancing

In order to adjust the intensities of colors in the image to look more realistic, I implemented automatic white balancing with the gray world assumption. Unlike our eyes, our images do not naturally white balance themselves! 

The gray world assumption assumes that all colors in an image will average out to a neutral gray, because the presence of any color will be balanced out by the presence of its complement. For most pictures, this is a reasonable assumption due to the distribution of colors in the image. White balancing involves two problems: first, estimating the illuminant, then manipulating the colors to counteract the illuminant and simulate a neutral illuminant. We estimate the illuminant by calculating the average RGB value in the image, then scale the color channels such that the new average RGB values are equal to the normalized values.

| Original | Balanced | 
|:-------------------------:|:-------------------------:|
|<img width="500" alt="cathedral" src="./cathedral_euclidian_sobel_gx 2_gy 5_rx 3_ry 12_ time 0.13 copy.jpg"> Cathedral, original | <img width="500" alt="cathedral" src="./white_balance_method_cathedral_euclidian_sobel_gx 2_gy 5_rx 3_ry 12_ time 0.13 copy.jpg"> Cathedral, white balanced |
|<img width="500" alt="self" src="./self_portrait_euclidian_sobel_gx 28_gy 77_rx 37_ry 176_ time 13.43 copy.jpg"> Self-Portrait, original | <img width="500" alt="self" src="./white_balance_method_self_portrait_euclidian_sobel_gx 28_gy 77_rx 37_ry 176_ time 13.43 copy.jpg"> Self-Portrait, white balanced|
|<img width="500" alt="lady" src="./lady_euclidian_sobel_gx 9_gy 56_rx 13_ry 119_ time 12.88 copy.jpg"> Lady, original | <img width="500" alt="lady" src="./white_balance_method_lady_euclidian_sobel_gx 9_gy 56_rx 13_ry 119_ time 12.88 copy.jpg"> Lady, white balanced |
|<img width="500" alt="church" src="./church_euclidian_sobel_gx 4_gy 25_rx -4_ry 58_ time 12.49 copy.jpg"> Church, original | <img width="500" alt="church" src="./white_balance_method_church_euclidian_sobel_gx 4_gy 25_rx -4_ry 58_ time 12.49 copy.jpg"> Church, white balanced |
|<img width="500" alt="melon" src="./melons_euclidian_sobel_gx 10_gy 80_rx 12_ry 177_ time 13.83 copy.jpg"> Church, original | <img width="500" alt="melon" src="./white_balance_method_melons_euclidian_sobel_gx 10_gy 80_rx 12_ry 177_ time 13.83 copy.jpg"> Church, white balanced |
|<img width="500" alt="icon" src="./icon_euclidian_sobel_gx 17_gy 42_rx 23_ry 90_ time 13.62 copy.jpg"> Church, original | <img width="500" alt="icon" src="./white_balance_method_icon_euclidian_sobel_gx 17_gy 42_rx 23_ry 90_ time 13.62 copy.jpg"> Church, white balanced |
|<img width="500" alt="carpet" src="./carpet_euclidian_sobel_gx -16_gy 48_rx -52_ry 91_ time 12.71 copy.jpg"> Carpet, original | <img width="500" alt="carpet" src="./white_balance_method_carpet_euclidian_sobel_gx -16_gy 48_rx -52_ry 91_ time 12.71 copy.jpg"> Carpet, white balanced |

Our white balancing algorithm indeed appears to work better on images with a wide variety of colors and an evenly distributed color spectrum. It performs worse when there are large areas of a single dominant color — cases where the average color is not gray.

## Automatic Cropping

In order to eliminate black and white borders from the resulting color images, I implemented an algorithm to automatically detect and remove these borders.


For each side of the picture, I selected the outermost column/row of pixels and averaged them to see if they fell within the threshold for close enough to 0 (black) or 1 (white). Since my images have pixel values in [0, 1], my threshold creates a range of acceptable values in [threshold, 1-threshold]. If the average pixel value of the selected group of pixels along a border falls outside of this range, a crop on that border occurs. I upper bounded the threshold by what the average value of a set of pixels would be if it was purely red, green, or blue, setting the threshold to 0.16. 

However, cropping one row of pixels at a time is very slow, especially for large images, and is not robust to outliers, like a single column of a different color in the middle of a solid black border. Instead, I took the average of a larger set of pixels along a border, checked it against the threshold, and if it fell within the croppable range I cropped off 2.5% of the image to reduce the number of comparisons needed. In the worst case of a faulty threshold comparison (such as if an image feature is too similar to an undesired border) or a thin border, a 2.5% crop is still a small adjustment and preserves the bulk of the image.

| Original | Cropped | 
|:-------------------------:|:-------------------------:|
|<img height="300" alt="church" src="./church_euclidian_sobel_gx 4_gy 25_rx -4_ry 58_ time 12.93 copy.jpg"> Church, original | <img height="300" alt="church" src="./cropped_church_euclidian_sobel_gx 4_gy 25_rx -4_ry 58_ time 12.93 copy.jpg"> Church, cropped |
|<img height="300" alt="tobolsk" src="./tobolsk_euclidian_sobel_gx 2_gy 3_rx 3_ry 6_ time 0.13 copy.jpg"> Tobolsk, original | <img height="300" alt="Tobolsk" src="./cropped_tobolsk_euclidian_sobel_gx 2_gy 3_rx 3_ry 6_ time 0.13 copy.jpg"> Tobolsk, cropped |
|<img height="300" alt="cathedral" src="./cathedral_euclidian_sobel_gx 2_gy 5_rx 3_ry 12_ time 0.17 copy.jpg"> Cathedral, original | <img height="300" alt="cathedral" src="./cropped_cathedral_euclidian_sobel_gx 2_gy 5_rx 3_ry 12_ time 0.17 copy.jpg"> Cathedral, cropped |
|<img height="300" alt="monastery" src="./monastery_euclidian_sobel_gx 2_gy -3_rx 2_ry 3_ time 0.11 copy.jpg"> Monastery, original | <img height="300" alt="monastery" src="./cropped_monastery_euclidian_sobel_gx 2_gy -3_rx 2_ry 3_ time 0.11 copy.jpg"> Monastery, cropped |

With more time, I would have tried this approach on segments of a particular side. Because artifacts distort the black and whiteness of the solid color borders, checking if a piece of the border falls within the threshold could potentially result in better crops.

`three_generations.tif` is a good example of an image that likely could have benefited from this approach:

| Original | Cropped | 
|:-------------------------:|:-------------------------:|
|<img height="300" alt="three_generations" src="./three_generations_euclidian_sobel_gx 12_gy 54_rx 8_ry 111_ time 12.83 copy.jpg"> Three Generations, original | <img height="300" alt="three gen" src="./cropped_three_generations_euclidian_sobel_gx 12_gy 54_rx 8_ry 111_ time 12.83 copy.jpg"> Three Generations, cropped |

The right border was not cropped because the average of that group of pixels was about 0.5, safely in the acceptable range. Further experimentation with higher thresholds might also improve automatic crops.

# All Together, Now!

Below is a selection of edge-aligned, white balanced, cropped images.

<table>
  <thead>
    <tr>
      <th colspan="2" style="text-align: center;">Gallery</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align: center;"><img width="1000" alt="melon" src="./white_balance_method_melons_euclidian_sobel_gx 10_gy 80_rx 12_ry 177_ time 13.46 copy.jpg">Melon </td>
      <td style="text-align: center;"><img width="1000" alt="emir" src="./white_balance_method_emir_euclidian_sobel_gx 23_gy 49_rx 40_ry 107_ time 11.1 copy.jpg">Emir</td>
    </tr>
     <tr>
      <td style="text-align: center;"><img width="1000" alt="church" src="./white_balance_method_church_euclidian_sobel_gx 4_gy 25_rx -4_ry 58_ time 12.01 copy.jpg">Church</td>
      <td style="text-align: center;"><img width="1000" alt="cathedral" src="./white_balance_method_cathedral_euclidian_sobel_gx 2_gy 5_rx 3_ry 12_ time 0.16 copy.jpg">Cathedral</td>
    </tr>
     <tr>
      <td style="text-align: center;"><img width="1000" alt="ditches" src="./white_balance_method_ditches_euclidian_sobel_gx -27_gy 33_rx -55_ry 74_ time 11.55 copy.jpg">Ditches</td>
      <td style="text-align: center;"><img width="1000" alt="forest" src="./white_balance_method_forest_euclidian_sobel_gx -41_gy 75_rx -69_ry 114_ time 12.29 copy.jpg">Forest</td>
    </tr>
         <tr>
      <td style="text-align: center;"><img width="1000" alt="barracks" src="./white_balance_method_barracks_euclidian_sobel_gx 1_gy 11_rx 0_ry 73_ time 12.98 copy.jpg">Barracks</td>
      <td style="text-align: center;"><img width="1000" alt="workshop" src="./white_balance_method_workshop_euclidian_sobel_gx 15_gy 73_rx 14_ry 154_ time 13.81 copy.jpg">Workshop</td>
    </tr>
  </tbody>
</table>


