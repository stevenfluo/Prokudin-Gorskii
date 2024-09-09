---
# Feel free to add content and custom Front Matter to this file.
# To modify the layout, see https://jekyllrb.com/docs/themes/#overriding-theme-defaults

layout: default
title: Home
permalink: /
---

# Overview


# Approach


I considered preprocessing images by cropping to the center 90% of each image (removing 5% of image size from each side), but instead used the center 90% as the image to score in order to prevent the edges from skewing the score computation while preserving the full image to allow for smarter cropping in the future.



# Results

<table>
  <thead>
    <tr>
      <th colspan="2" style="text-align: center;">Low Quality Images (.jpg)</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align: center;"><img width="1000" alt="cathedral" src="./rgb_base_results/cathedral_euclidian_rgb_gx 2_gy 5_rx 3_ry 12_ time 0.13.jpg"> Displacement: G (2, 5); R (3, 12) <br /> Runtime: 0.13 seconds</td>
      <td style="text-align: center;"><img width="1000" alt="monastery" src="./rgb_base_results/monastery_euclidian_rgb_gx 2_gy -3_rx 2_ry 3_ time 0.11.jpg"> Displacement: G (2, -3); R (2, 3) <br /> Runtime: 0.11 seconds</td>
    </tr>
    <tr>
      <td style="text-align: center;"><img width="1000" alt="tobolsk" src="./rgb_base_results/tobolsk_euclidian_rgb_gx 2_gy 3_rx 3_ry 6_ time 0.12.jpg"> Displacement: G (2, 3); R (3, 6) <br /> Runtime: 0.12 seconds</td>
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
      <td style="text-align: center;"><img width="1000" alt="church" src="./rgb_base_results/church_euclidian_rgb_gx 3_gy 25_rx -5_ry 58_ time 10.94.jpg">Displacement: G (3, 25); R (-5, 58) <br /> Runtime: 10.94 seconds</td>
      <td style="text-align: center;"><img width="1000" alt="emir" src="./rgb_base_results/emir_euclidian_rgb_gx 24_gy 49_rx 43_ry 88_ time 10.41.jpg">Displacement: G (24, 49); R (43, 88) <br /> Runtime: 10.41 seconds</td>
    </tr>
    <tr>
      <td style="text-align: center;"><img width="1000" alt="harvesters" src="./rgb_base_results/harvesters_euclidian_rgb_gx 16_gy 60_rx 13_ry 124_ time 10.38.jpg">Displacement: G (16, 60); R (13, 24) <br /> Runtime: 10.38 seconds</td>
      <td style="text-align: center;"><img width="1000" alt="icon" src="./rgb_base_results/icon_euclidian_rgb_gx 17_gy 41_rx 23_ry 89_ time 11.21.jpg">Displacement: G (17, 41); R (23, 89) <br /> Runtime: 11.21 seconds</td>
    </tr>
    <tr>
      <td style="text-align: center;"><img width="1000" alt="lady" src="./rgb_base_results/lady_euclidian_rgb_gx 8_gy 55_rx 12_ry 111_ time 10.49.jpg">Displacement: G (8, 55); R (12, 111) <br /> Runtime: 10.49 seconds</td>
      <td style="text-align: center;"><img width="1000" alt="melons" src="./rgb_base_results/melons_euclidian_rgb_gx 9_gy 82_rx 11_ry 177_ time 11.0.jpg">Displacement: G (9, 82); R (11, 177) <br /> Runtime: 11.00 seconds</td>
    </tr>
    <tr>
      <td style="text-align: center;"><img width="1000" alt="onion" src="./rgb_base_results/onion_church_euclidian_rgb_gx 26_gy 51_rx 36_ry 108_ time 11.31.jpg">Displacement: G (26, 51); R (36, 108) <br /> Runtime: 11.31 seconds</td>
      <td style="text-align: center;"><img width="1000" alt="sculpture" src="./rgb_base_results/sculpture_euclidian_rgb_gx -11_gy 33_rx -27_ry 140_ time 11.02.jpg">Displacement: G (-11, 33); R (-27, 140) <br /> Runtime: 11.02 seconds</td>
    </tr>
    <tr>
      <td style="text-align: center;"><img width="1000" alt="self portrait" src="./rgb_base_results/self_portrait_euclidian_rgb_gx 29_gy 79_rx 34_ry 175_ time 11.83.jpg">Displacement: G (29, 79); R (34, 175) <br /> Runtime: 11.83 seconds</td>
      <td style="text-align: center;"><img width="1000" alt="three gen" src="./rgb_base_results/three_generations_euclidian_rgb_gx 13_gy 55_rx 10_ry 112_ time 11.52.jpg">Displacement: G (13, 55); R (10, 112) <br /> Runtime: 11.52 seconds</td>
    </tr>
    <tr>
      <td style="text-align: center;"><img width="1000" alt="train" src="./rgb_base_results/train_euclidian_rgb_gx 6_gy 42_rx 32_ry 87_ time 11.8.jpg">Displacement: G (6, 42); R (32, 87) <br /> Runtime: 11.8 seconds</td>
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

Introduction and Why does it work?

This example shows the clear improvement in `emir.tif` when aligning based on RGB similarity to aligning based on edges:

| RGB | Edge | 
|:-------------------------:|:-------------------------:|
|<img width="1000" alt="emir rgb" src="./rgb_base_results/emir_euclidian_rgb_gx 24_gy 49_rx 43_ry 88_ time 10.41.jpg"> Displacement: G (24, 49); R (43, 88) <br /> Runtime: 10.41 seconds | <img width="1000" alt="emir edges" src="./sobel_base_results/emir_euclidian_sobel_gx 23_gy 49_rx 40_ry 107_ time 11.36.jpg"> Displacement: G (23, 49); R (40, 107) <br /> Runtime: 11.36 seconds|


## Better Scoring




## Just For Fun
I was curious about what other photos the Library of Congress had in their collection (and if my code would work on these images) so I downloaded 15 more images from the catalog that caught my eye, and that seemed hard to align. In my comparisons using this set of images, I found my above findings held on this new set of images.

For example, using edges as a feature significantly improved alignment on tricky images, especially when the brightness on the three color channel images were not consistent. This example shows the clear improvement in `yurt.tif` when aligning based on RGB similarity to aligning based on edges:

| RGB | Edge | 
|:-------------------------:|:-------------------------:|
|<img width="1000" alt="yurt rgb" src="./exrgb_results/yurt_euclidian_rgb_gx 38_gy 48_rx -9_ry 115_ time 10.57.jpg"> Displacement: G (38, 48); R (-9, 115) <br /> Runtime: 10.57 seconds | <img width="1000" alt="yurt edges" src="./exedge_results/yurt_euclidian_sobel_gx 37_gy 53_rx 56_ry 107_ time 12.5.jpg"> Displacement: G (37, 57); R (56, 107) <br /> Runtime: 12.5 seconds|

A randomly selected sample of edge-aligned results are found in the following table:

<table>
  <thead>
    <tr>
      <th colspan="2" style="text-align: center;">Full Quality Images (.tif)</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align: center;"><img width="1000" alt="workshop" src="./exedge_results/workshop_euclidian_sobel_gx 15_gy 73_rx 14_ry 154_ time 13.3.jpg">Displacement: G (15, 73); R (14, 154) <br /> Runtime: 13.3 seconds</td>
      <td style="text-align: center;"><img width="1000" alt="greenhouse" src="./exedge_results/greenhouse_euclidian_sobel_gx 28_gy 59_rx 34_ry 126_ time 12.6.jpg">Displacement: G (28, 59); R (24, 126) <br /> Runtime: 12.6 seconds</td>
    </tr>
    <tr>
      <td style="text-align: center;"><img width="1000" alt="ditches" src="./exedge_results/ditches_euclidian_sobel_gx -27_gy 33_rx -55_ry 74_ time 10.85.jpg">Displacement: G (-27, 33); R (-55, 74) <br /> Runtime: 10.85 seconds</td>
      <td style="text-align: center;"><img width="1000" alt="vases" src="./exedge_results/vases_euclidian_sobel_gx 5_gy 25_rx 9_ry 110_ time 11.69.jpg">Displacement: G (5, 25); R (9, 110) <br /> Runtime: 11.69 seconds</td>
    </tr>
    <tr>
      <td style="text-align: center;"><img width="1000" alt="forest" src="./exedge_results/forest_euclidian_sobel_gx -41_gy 75_rx -69_ry 114_ time 11.64.jpg">Displacement: G (-41, 75); R (-69, 114) <br /> Runtime: 11.64 seconds</td>
      <td style="text-align: center;"><img width="1000" alt="sunset" src="./exedge_results/sunset_euclidian_sobel_gx 2_gy 52_rx -35_ry 119_ time 11.28.jpg">Displacement: G (2, 52); R (-35, 119) <br /> Runtime: 11.28 seconds</td>
    </tr>
    <tr>
      <td style="text-align: center;"><img width="1000" alt="lugano" src="./exedge_results/lugano_euclidian_sobel_gx -17_gy 41_rx -29_ry 91_ time 12.35.jpg">Displacement: G (-17, 41); R (-29, 91) <br /> Runtime: 12.35 seconds</td>
      <td style="text-align: center;"><img width="1000" alt="poppies" src="./exedge_results/poppies_euclidian_sobel_gx -1_gy 12_rx -3_ry 98_ time 11.15.jpg">Displacement: G (-1, 12); R (-3, 98) <br /> Runtime: 11.15 seconds</td>
    </tr>
    <tr>
      <td style="text-align: center;"><img width="1000" alt="carpet" src="./exedge_results/carpet_euclidian_sobel_gx -16_gy 48_rx -52_ry 91_ time 12.12.jpg">Displacement: G (-16, 48); R (-52, 91) <br /> Runtime: 12.12 seconds</td>
      <td style="text-align: center;"><img width="1000" alt="italy" src="./exedge_results/italy_euclidian_sobel_gx 22_gy 38_rx 36_ry 77_ time 12.1.jpg">Displacement: G (22, 38); R (36, 77) <br /> Runtime: 12.1 seconds</td>
    </tr>
    <tr>
      <td style="text-align: center;"><img width="1000" alt="lilacs" src="./exedge_results/lilacs_euclidian_sobel_gx -11_gy 43_rx -36_ry 95_ time 12.35.jpg">Displacement: G (-11, 43); R (-36, 95) <br /> Runtime: 12.35 seconds</td>
      <td style="text-align: center;"><img width="1000" alt="peonies" src="./exedge_results/peony_euclidian_sobel_gx 20_gy 76_rx 31_ry 155_ time 13.42.jpg">Displacement: G (20, 76); R (31, 155) <br /> Runtime: 13.42 seconds</td>
    </tr>
    <tr>
      <td style="text-align: center;"><img width="1000" alt="hill" src="./exedge_results/hill_euclidian_sobel_gx 5_gy 39_rx 4_ry 130_ time 12.03.jpg">Displacement: G (5, 39); R (4, 130) <br /> Runtime: 12.03 seconds</td>
      <td style="text-align: center;"><img width="1000" alt="barracks" src="./exedge_results/barracks_euclidian_sobel_gx 1_gy 11_rx 0_ry 73_ time 11.22.jpg">Displacement: G (1, 11); R (0, 73) <br /> Runtime: 11.22 seconds</td>
    </tr>
  </tbody>
</table>

