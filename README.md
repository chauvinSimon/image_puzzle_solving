# image_puzzle_solving

| ![examples of puzzle solving](docs/success-agent.gif "examples of puzzle solving")  | 
|:--:| 
| *examples of puzzle solving* |


## Assumptions:
- constant width of colums (two neighbours may be already next to each other)
- constant height of rows (two neighbours may be already next to each other)
- .png colour pictures

## Tools:
- a **"fitness"** is defined for a pair of pieces
    - it represents the sum of **discountinuity** in the three colour channels when crossing the border
- a **level** represents a group of pieces equaly distant to the top-left corner

| ![conventions of levels](docs/level_conventions.PNG "conventions of levels")  | 
|:--:| 
| *conventions of levels* |


## Steps:
- determine the **number of rows** and **number of columns**
    - discountinuities between two consecutive lines of pixels are collected
    - a threshold is applied: *it must be hand-tuned*
    - given the "constant width" and "constant height" assumption, a **Greatest Common Divisor** is used to infer the most plausible number of rows and columns
- decompose the picture in pieces based on the inferred dimensions
- compute **two fitness matrices**
    - they give for each pair of pieces the discontinuity
    - for the `left->right` and `top->down` relations
- model the problem
    - Let **each piece** be a **node in a graph**. Let connect each pair of pieces by **four directed vertices**.
        - Two representing the horizontal discountinuity (p1->p2 and p2->p1) and two for the vertical discountinuity.
        - the **graph is complete**, hence the existence of an **Hamiltonian Path** is ensured.
    - The problem now boils down to **finding a path** that
        - **visits exactly once** each piece of the puzzle
        - while having the **lowest total cost** (sum of discountinuity measures at each transition)
        - while respecting the **shape of the image**: `n_rows * n_cols`
- instead of these **global optimization**, a **greedy approach** is taken    
    - find best piece for the upper left corner: the worse in term of right-neighbouring and down-neighbouring
    - iterate per "level":
        - for position, find the **two parents** (upper and left neighbours) that belong to the previous level
        - among the available pieces, choose the one that **fits the most**: lowest sum of horizontal and vertical discountinuity

| ![naming conventions for fitness matrix](docs/matrix_convention.PNG "naming conventions for fitness matrix")  | 
|:--:| 
| *naming conventions for fitness matrix* |


| ![threshold tuning to get n_cols and _rows](docs/threshold_tuning.PNG "threshold tuning to get n_cols and _rows")  | 
|:--:| 
| *threshold tuning to get n_cols and _rows* |


### getting started

- to install Pillow:
 
 - `> pip install Pillow`

- run [`image_puzzle_solving.ipynb`](https://github.com/chauvinSimon/image_puzzle_solving/blob/master/image_puzzle_solving.ipynb)

### acknowledgement

triathlon pictures by ITU