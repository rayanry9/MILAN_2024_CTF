---
layout: post
title: "Cool Matrices"
date: 2024-09-25
categories: Miscellaneous
---

## Problem 

![Problem](../../../../assets/cool-matrices-1.png)

We are only given a zip file for this challenge.

## Solution

+ I use `binwalk` on the `chall.zip` to find if any hidden files are there are not.

    ![](../../../../assets/cool-matrices-2.png)

    We see that there are only two files compressed in the zip.
    So, no hidden files here.

+ Uncompressing the files, result in two files `matrix1.txt` and `matrix2.txt` 
    
+ According to the problem, there is some information stored in `matrix1.txt` regarding the operations done on `matrix2.txt`

+ Upon observing `matrix1.txt` carefully, the matrix mostly consists of the number `255` and very few cases of other numbers.
    
    @Chaitanya observed this and converted `matrix1.txt` into a proper `csv` file and imported it into a spreedsheet.
    Then, he added a _conditional color format_ to color the cells which dont have 255 value.
    
    ![](../../../../assets/cool-matrices-3.png)
    
    The resultant data turns out to be 
        
    - `A`<sub>ij</sub> denotes the element in `i`<sup>th</sup> row `1<= i <= n` and `j`<sup>th</sup> `(1 <= j <=m )` column in the matrix A of size `n x m`
    - Operation `(1, a, b)` : For all `1 <= j <= m` set `A`<sub>aj</sub> `= (A`<sub>aj</sub>` + A`<sub>bj</sub>`)%256`
    - Operation `(2, a, b)` : For all `1 <= j <= m` set `A`<sub>aj</sub> `= (b x A`<sub>aj</sub>`)%256`
    - Operation List :
        I remember storing the oprations I performed somewhere here too...

+ After getting the operations, we had to find the list of operations.

    This was easily found out by just observing for red color cells in the excel.

    ![](../../../../assets/cool-matrices-4.png)

+ Now as `matrix2.txt` contains data that has already been operated on.

    - I collected the operations data.
    - Put it in reverse order
    - Reversed the calculation for both the operations.
    - Calculated original `matrix2` using reversed operations.

    - I wrote a cpp function to do the same. 

    ```{c}
    #include <cmath>
    #include <iostream>

    int main() {
        const int size = 287;
        int matrix[size][size];

        for (int i = 0; i < size; i++) {
            for (int j = 0; j < size; j++) {
                std::cin >> matrix[i][j];
            }
        }

        int t = 17;
        while (t--) {

        int o, a, b;
        std::cin >> o >> a >> b;

        switch (o) {
            case 1:
                for (int j = 0; j < size; j++) {

                    if (matrix[a][j] - matrix[b][j] > 0) {
                        matrix[a][j] = matrix[a][j] - matrix[b][j];
                    } else {
                        matrix[a][j] = matrix[a][j] - matrix[b][j] + 256;
                    }
                }
                break;
            case 2:
                for (int j = 0; j < size; j++) {
                    int k = 0;
                        while (true) {
                        if (floor(matrix[a][j] + (k * 256.0) / b) == ceil(matrix[a][j] + (k * 256.0) / b)) {
                            matrix[a][j] = floor(matrix[a][j] + (k * 256.0) / b);
                            break;
                        }
                    }
                }
                break;
            }
        }

        for (int i = 0; i < size; i++) {
            for (int j = 0; j < size; j++) {
                std::cout << matrix[i][j] << " ";
            }
            std::cout << '\n';
        }
    }   

    ```

+ The code gave a new matrix \( the original one \).

+ As the values of each element in the matrix was between 0 and 255, I converted it to an grayscale image using the value as its luminance using a generic python script made using ChatGPT.

![](../../../../assets/cool-matrices-5.png)

+ The flag can be read out of the following image : milanCTF{m4tr1c3s_t0_1m4ge5} 
