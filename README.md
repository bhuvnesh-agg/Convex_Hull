# Convex Hull

![hull](https://github.com/user-attachments/assets/061bdbdc-794c-43c7-977c-6b2bee591a49)


## Overview
This repository contains the design and implementation of a digital circuit that computes the convex hull of a given set of points using the Jarvis March algorithm. 
The points are stored in ROM for efficient access.

## Circuit Description
### Jarvis's March Algorithm
Jarvis March is a simple algorithm to find the convex hull of a set of points. A convex hull is the smallest convex polygon that contains all the points.

In this circuit, Jarvis's March is implemented as follows:

1. **Iteration:** Three counters are used for iteration through the ROM: a MOD-32 counter(`COUNTER`) that provides the index to the ROM, a MOD-3 counter(`MOD3_COUNTER`) that tells wether the current number being accessed is the index, X-coordinate or Y-coordinate of the current point, and finally a MOD-16 counter(`INDEX`) that stores the point index as long as the next point index is not encountered.
In addition, a loop-end condition (`LOOPEND`) is also evaluated that resets the counters as soon as the end of the array is encountered.

2. **Finding the Leftmost Point:** The `LEFTMOST` sub-circuit calculates the leftmost point by comparing the x coordinate of each point with the previous. By the end of the first loop, we get the leftmost point

3. **Generating the Convex Hull:** This is done inside the `JARVIS MARCH` sub-circuit. Here, we take 3 points: A, B and C. A stores the latest point found on the convex hull (initially, this is the leftmost point); B stores the next point that will be shifted to A; C iterates through every other point. The orientation of these 3 points is calculated and if it turns out to be counter clockwise, C is immediately shifted to B and C is incremented to the next point. At the end of the loop, B is shifted to A and then B is set to the next point. This is repeated till A again equals the leftmost point.

4. **Generating the Hull Sum:** The convex hull sum is the sum of the index of all points lying on the convex hull. This is calculated along step 3.

5. **Displaying the Hull Sum and Current Hull Point:** To display the sum using 7 segment displays, a simple binary-to-BCD converter was made.

## Usage
- Create a `.bin` file containing the point set in the form: $N, i_1, x_1, y_1, i_2, x_2, y_2 ...$
  > Here, N is the total number of points and each point k has a unique index($i_k$), an X co-ordinate ($x_k$), and a Y co-ordinate ($y_k$).
- Open the `.pdsprj` file in Proteus EDA Software.
- Select the ROM Module (2732) and edit properties
- Change `Image File: ` field to the file created above.
- The final convex hull sum and the points will be visible on the 7 segment displays on running the simulation.
