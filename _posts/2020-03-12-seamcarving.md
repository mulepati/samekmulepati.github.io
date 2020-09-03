---
title: "Content Aware Image Resizing"
date: 2020-02-24
tags: [Pathfinder, Java]
header:
excerpt: "Attempted to create an image resizing program, based on a 2007 pathfinding algorithm"
mathjax: "true"
---
# Seam Carving (Content Aware Image Resizing)

## Inspiration:
This was a class project where we attempted to learn how to solve real-world graph problems via reduction. To do this we utilized an algorithm discoved in 2007 inorder to implmenet an image resizing program that is content aware. This mean that when we vertiacally or horizontally resize an image it will only remove pixels that have the least amount of information present with in them, or only add pixels back in based on where it will have the least effect on the overall image.

[Video demo](https://www.youtube.com/watch?v=6NcIJXTlugc)

## What it does:

#### Energy Calculation

The program first creates an energy map of the image utilizing the dual-gradient engergy function below. Converting an image like this:
![image1](https://courses.cs.washington.edu/courses/cse332/20wi/assets/images/seamcarving-HJoceanSmall.png)
 into and energy graph which would look like this:
![image2](https://courses.cs.washington.edu/courses/cse332/20wi/assets/images/seamcarving-HJoceanSmallEnergy.png)

**Dual-Gradient Energy Function **
![function1](https://latex.codecogs.com/gif.latex?energy(x,&space;y)&space;=&space;\sqrt&space;(\nabla_x^2&space;(x,&space;y)&space;&plus;&space;\nabla_y^2(x,&space;y))%22%20title=%22energy(x,%20y)%20=%20\sqrt%20(\nabla_x^2%20(x,%20y)%20+%20\nabla_y^2(x,%20y)))

Where the square of the x-gradient is Red(x, y)^2 + Green(x,y)^2 + Blue(x,y)^2 and same for the y-gradient. Since each image is represented by a matrix of pixals, where each pixal has a value for red, green, and blue inorder to determine its color. The following function takes the image and creates a diffirent matrix that has only has one value for each pixal representing the energy of the given pixal. 

|  (78, 209, 79)  |  (63, 118, 247) |  (92, 175, 95)  |  (243, 73, 183) | (210, 109, 104) | (252, 101, 119) |
|:---------------:|:---------------:|:---------------:|:---------------:|:---------------:|:---------------:|
| (224, 191, 182) |  (108, 89, 82)  |  (80, 196, 230) | (112, 156, 180) | (176, 178, 120) | (142, 151, 142) |
| (117, 189, 149) | (171, 231, 153) | (149, 164, 168) |  (107, 119, 71) | (120, 105, 138) | (163, 174, 196) |
| (163, 222, 132) | (187, 117, 183) |  (92, 145, 69)  |  (158, 143, 79) |  (220, 75, 222) |  (189, 73, 214) |
| (211, 120, 173) | (188, 218, 244) |  (214, 103, 68) | (163, 166, 246) |  (79, 125, 246) |  (211, 201, 98) |

So given an image that can be represented the graph above, the energy converstion step converts it into a table like this.

| 240.18 | 225.59 | 302.27 | 159.43 | 181.81 | 192.99 |
|:------:|:------:|:------:|:------:|:------:|:------:|
| 124.18 | 237.35 | 151.02 | 234.09 | 107.89 | 159.67 |
| 111.10 | 138.69 | 228.10 | 133.07 | 211.51 | 143.75 |
| 130.67 | 153.88 | 174.01 | 284.01 | 194.50 | 213.53 |
| 179.82 | 175.49 |  70.06 | 270.80 | 201.53 | 191.20 |

#### Seam identification

After the energy map has been calculated, we find the minimum vertical or horizontal seam based on how we are resizing. In order to find a vertical or horizontal seam we create a DAG out of the energy matrix. 

For an example fo the vertical DAG, each pixal is connected to the pixal above it, the pixal above and to the right,a nd above and to the left. All the pixals at the top of the matrix is connected to a Sink vertex, and all the pixals at the bottom of the matrix is connected to a source vertex. Each vertex has the weight of the value in the energy matrix. We find the lowest energy path from the source to the sink, which would represent the vertical seam that can be removed or duplicated.

For the given graph a seam would be the following path.

| 0 |0 |0 |0 | 0 | 0 |
|:------:|:------:|:------:|:------:|:------:|:------:|
| 240.18 | 225.59 | 302.27 | **159.43** | 181.81 | 192.99 |
| 124.18 | 237.35 | 151.02 | 234.09 | **107.89** | 159.67 |
| 111.10 | 138.69 | 228.10 | **133.07** | 211.51 | 143.75 |
| 130.67 | 153.88 | **174.01** | 284.01 | 194.50 | 213.53 |
| 179.82 | 175.49 |  **70.06** | 270.80 | 201.53 | 191.20 |

A seam can be seen in the following image as a red line:

![image3](https://courses.cs.washington.edu/courses/cse332/20wi/assets/images/seamcarving-HJoceanSmallVerticalSeam.png)

## How I built it:

This program was build using Java, and simple Java GUI to represent the images as well. There were multiple classes and components, each modularly tested and put together at the end. 

## How to try it out:

Contant me through email to get access to the following code. Access has been restricted since the project is an ongoing homework project for CSE 332 Data Structures and Parallism class at UW Seattle.

## Sources

Josh Hug. 2015. Seam Carving. In Nifty Assignments 2015. http://nifty.stanford.edu/2015/hug-seam-carving/

Josh Hug, Kevin Wayne, and Maia Ginsburg. 2019. Seam Carving. In COS 226, Spring 2019. https://www.cs.princeton.edu/courses/archive/spring19/cos226/assignments/seam/specification.php ↩
