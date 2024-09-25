---
layout: post
title: The Real CTF Game
date: 2024-09-25
categories: miscellaenous
---

# Problem

![Problem](../../../../assets/rcg-1.png)

In this challenge, we have to get the flag from the server by connecting to it with `netcat`.
We have been provided with the `domain` and `port` of the server along with the `c` code that is being run on the server for local testing.

# Solution

+ By reading the code, it is understandable that we have to move through a grid and search for a flag in the grid.
+ When we reach the flag, we have to type 'y'.
+ To win the game, we have to reach the end of the grid - 456,456

+ #### Code Given

```{c}

#include <stdio.h>
#include <time.h> #include <stdlib.h>
#define MAP_WIDTH 456
#define MAP_HEIGHT 456

// Function to read input and skip newline characters
char get_valid_input() {
    char input;
    do {
        input = getchar();
    } while (input == '\n');  // Ignore newline characters
    return input;
}

int main() {
    printf("Welcome to the game of CTF!\n");
    printf("You can move using the 'w', 'a', 's', 'd' keys, 'q' to quit.\n");
    printf("Somewhere in the grid, the flag is hidden.\nCan you find your way through the game?\n\n");
    fflush(stdout); // Ensure immediate output flush

    char map[MAP_HEIGHT][MAP_WIDTH];
    for (int y = 0; y < MAP_HEIGHT; y++) {
        for (int x = 0; x < MAP_WIDTH; x++) {
            map[y][x] = '.';
        }
    }

    int keyX, keyY, win = 0;
    srand(time(NULL));
    keyX = rand() % (MAP_HEIGHT - 150) + 75;
    keyY = rand() % (MAP_WIDTH - 150) + 75;
    map[MAP_HEIGHT - 1][MAP_WIDTH - 1] = 'S';

    int playerX = 0;
    int playerY = 0;

    char input, input2;
    printf("Player Position: %d, %d\n", playerX, playerY);
    printf("Player has the flag: %d\n", win);
    printf("Enter your first move:\n");
    fflush(stdout); // Flush output

    while (1) {
        input = get_valid_input();  // Get valid input, skipping newlines

        switch (input) {
            case 'w':
                if (playerY > 0) playerY--;
                break;
            case 's': 
                if (playerY < MAP_HEIGHT - 1) playerY++;
                else if (playerX == MAP_WIDTH - 1) {
                    playerX = 0;
                    playerY = 0;
                } else {
                    playerX++;
                    playerY = 0;
                }
                break;
            case 'a': 
                if (playerX > 0) playerX--;
                break;
            case 'd':
                if (playerX < MAP_WIDTH - 1) playerX++;
                break;
            case 'q':
                return 0;
            default:
                printf("Enter a valid move\n");
                fflush(stdout);  // Flush output
                continue; // Skip to next loop iteration
        }

        printf("Player Position: %d, %d\n", playerX, playerY);
        printf("Player has the flag: %d\n", win);
        fflush(stdout); // Flush output after each move

        if (playerX == keyX && playerY == keyY) {
            if (win == 0) {
                printf("Press 'y' to capture the flag.\n");
                fflush(stdout);
                input2 = get_valid_input();  // Capture flag confirmation
                if (input2 == 'y') {
                    win = 1;
                    printf("Congratulations! You've captured the flag.\n");
                    fflush(stdout);
                } else {
                    printf("You missed the flag.\n");
                    fflush(stdout);
                }
            } else {
                printf("You've already captured the flag.\n");
                fflush(stdout);
            }
        }

        if (playerX == MAP_WIDTH - 1 && playerY == MAP_HEIGHT - 1) {
            if (win == 1) {
                printf("Congratulations! You won the game!\n");
                printf("milanCTF{-REDACTED-}\n");
                fflush(stdout);
                break;
            } else {
                printf("OH No! You don't have the flag.\n");
                fflush(stdout);
            }
        }
    }

    return 0;
}

```

+ As the number of inputs is way toooo large, I made a `bash` script to output the inputs in a file.
+ The inputs are in the format of `movement + 'y'`

+ It outputs `y` after each movement so that if at any point it reaches the flag coordinates, it can capture the flag and not go over it.

+ My Bash Script

```{bash}
#!/bin/bash

for i in {1..456}
do
    for (( j=1; j<=455; j++ ))
        do
            if (( $i % 2 == 0 ))
            then
                echo "d" >> "./input";
            else
                echo "a">> "./input";
            fi
            echo "y" >> "./input";
        done
    echo "s" >> "./input";
    echo "y" >> "./input";
done
```
+ I compile the given `c` code in the problem to `a.out` and run `cat input`\|`./a.out > output` to pipe the entire bash output to the `c` code, and pipe the `c` output to `output` file.

![](../../../../assets/rcg-2.png)

+ Next, open the file in vim and type `:%s/milan` to find the flag through all the text`.

![](../../../../assets/rcg-3.png)

+ This results in me getting the flag in local testing.

+ The same thing is repeated on the server.

+ I call `cat input`\|`netcat milanctf.kludge.co.in 4560 > output`
+ I get the server output in the file `output`

+ Next, I open the file in vim and type `:%s/milan` to find the flag through all the text`.

![](../../../../assets/rcg-4.png)

+ The flag is milanCTF{D0_y0u_r3m3b3r_sQuiD_g4m3_456}


