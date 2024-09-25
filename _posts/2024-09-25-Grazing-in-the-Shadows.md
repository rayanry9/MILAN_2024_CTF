---
layout: post
title: Grazing in the Shadows 
date: 2024-09-25
categories: reverseEngineering
---

# Problem

![](../../../../assets/gits-1.png)

In this challenge, we get a zip file, which contains two files `destruction_plan.txt` and `farm_info.txt`
The question helps us understand that we have to view the field after the cows graze it.


# Solution

+ The Question itself is not tricky.
+ It is a matter of using the algorithm given in `destruction_plan.txt` or understanding and make the cows graze the field.

+ `destruction_plan.txt` ->

```
# Farm destruction plan

MAX_TIME = 1000000

set farm_info.txt as input file

read N, M // size of farm
read K // number of cows

isGrazed[] = {false} // indicates if a block is grazed, size N * M

# position of cows
for i = 1 -> K
    read x, y
    cows[i] = Cow(x, y)
end

edges = []
for i = 1 -> N
    for j = 1 -> M
        for each neighbour v of (i, j)
            edges[(i, j) <-> v] = Edge((i, j) <-> v)
            edges[(i, j) <-> v].lock()
        end
    end
end

for i = 1 -> N-1
    for j = 1 -> M
        read x
        edges[(i, j) <-> (i+1, j)].unlockTime = x
    end
end

for i = 1 -> N
    for j = 1 -> M-1
        read x
        edges[(i, j) <-> (i, j+1)].unlockTime = x
    end
end

for time = 0 -> MAX_TIME
    for each block u in grid
        for each neighbour v
            if edges[u<->v].unlockTime == time
                edges[u<->v].unlock()
            end
        end
    end
    for each cow c in cows
        for each block u in grid
            if c.canReach(u)
                isGrazed[u] = true
            end
        end
    end
end

# This is how we shall deprive the farm of its greenery and make it barren.
# Can't wait to see how the barren farm will look like!


```

+ Understanding this, I made the code to simulate the logic of cows.
+ `code.cpp`:

```{c}
#include <iostream>
#include <set>
#include <vector>

struct Cow {
  long x{};
  long y{};
};

// Union-Find data structure for managing group IDs
class UnionFind {
public:
  UnionFind(long size) : parent(size), rank(size, 0) {
    for (long i = 0; i < size; i++) {
      parent[i] = i;
    }
  }

  long find(long u) {
    if (parent[u] != u) {
      parent[u] = find(parent[u]); // Path compression
    }
    return parent[u];
  }

  void unite(long u, long v) {
    long rootU = find(u);
    long rootV = find(v);
    if (rootU != rootV) {
      if (rank[rootU] < rank[rootV]) {
        parent[rootU] = rootV;
      } else if (rank[rootU] > rank[rootV]) {
        parent[rootV] = rootU;
      } else {
        parent[rootV] = rootU;
        rank[rootU]++;
      }
    }
  }

private:
  std::vector<long> parent;
  std::vector<long> rank;
};

int main() {
  long MAX_TIME = 1000000;
  long N, M, K;
  std::cin >> N >> M >> K;

  Cow cows[K];
  for (long i = 0; i < K; i++) {
    std::cin >> cows[i].x >> cows[i].y; // Reading cow positions
  }

  // Initialize Union-Find structure
  UnionFind uf(N * M);
  std::vector<std::vector<long>> horizontalEdges(N - 1,
                                                 std::vector<long>(M, 0));
  std::vector<std::vector<long>> verticalEdges(N, std::vector<long>(M - 1, 0));

  // Process horizontal edges
  for (long i = 0; i < N - 1; i++) {
    for (long j = 0; j < M; j++) {
      long time;
      std::cin >> time;
      if (time <= MAX_TIME) {
        horizontalEdges[i][j] = 1;
        uf.unite(i * M + j, (i + 1) * M + j); // Connect (i, j) with (i + 1, j)
      }
    }
  }

  // Process vertical edges
  for (long i = 0; i < N; i++) {
    for (long j = 0; j < M - 1; j++) {
      long time;
      std::cin >> time;
      if (time <= MAX_TIME) {
        verticalEdges[i][j] = 1;
        uf.unite(i * M + j, i * M + (j + 1)); // Connect (i, j) with (i, j + 1)
      }
    }
  }

  // Set to track which groups can be grazed
  std::set<long> toGraze;
  for (long i = 0; i < K; i++) {
    long cowGroupID = uf.find((cows[i].x - 1) * M + (cows[i].y - 1));
    toGraze.insert(cowGroupID);
  }

  // Final grazing output
  for (long i = 0; i < N; i++) {
    for (long j = 0; j < M; j++) {
      long cellGroupID = uf.find(i * M + j);
      if (toGraze.find(cellGroupID) != toGraze.end()) {
        std::cout << 1 << ' '; // Mark as grazed
      } else {
        std::cout << 0 << ' '; // Mark as not grazed
      }
    }
    std::cout << '\n';
  }

  return 0;
}
```

+ Compiling `code.cpp` to `a.out`
+ I Run `cat farm_info.txt`\|`./a.out > output`

+ This results in a space separated 0's and 1's file.
+ Take 1 as white and 0 as black, a simple python script is run to output the image.

![](../../../../assets/gits-2.png)

+ Thus, we get the key = milanCTF{DSA_c4nt_b3at_RSA_256512}
