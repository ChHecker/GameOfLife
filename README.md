# Game of Life
This library is my attempt at implementing an advanced Conway's Game of Life in Rust. My goal was to optimize and overengineer it in order to learn Rust along the way.

![Game of Life demo](demo.gif)

Conway's Game of Life (in my advanced version) plays in a grid containing cells (integers) that are either dead (0) or alive (>0). Then, the next generation is calculated with the prior time step following rule:
1. A cell is alive if its value is `state`.
2. Neighbors are either all eight surrounding cell (`Moore`) or only the four adjacent cells (`VonNeumann`).
3. If a cell was dead, it will be revived if it had a number of living neighbors specified in `birth`.
4. If a cell was alive, it will stay alive if it had a number of living neighbors specified in `survive`. If that's not the case, its value will decrease by one.

## Usage
You can compile the program with `cargo build --release`. The binary then takes the following syntax:  
```gameoflife -{flags} {gif|tui}```  
- `gif` saves the Game of Life as a GIF with [`plotters`](https://docs.rs/plotters/latest/plotters/),  
- `tui` plots the Game of Life in the terminal with [`termion`](https://crates.io/crates/termion)  
- no command starts a dialogue that will ask about all parameters with sane defaults
The flags can be:
- `-a`: algorithm (std or conv)
- `-n`: neighbor algorithm (m or vn)
- `-i`: number of iterations
- `-x`: number of columns
- `-y`: number of rows
- `-p`: probability of a cell being alive in the initial field
- `-s`: state
- `-h`: list all commands  

## Algorithms
The standard algorithms iterates over every cell, counts its neighbors, and then decides whether it's alive in the next step via normal `if` statements. This is fairly quick, especially as I used [`rayon`](https://crates.io/crates/rayon) to do this with multiple threads.  
The convolution algorithm, however, is about 17x faster. It uses [`ndarray-ndimage`](https://crates.io/crates/ndarray-ndimage) to convolve the field with the kernel `[[1, 1, 1], [1, 0, 1], [1, 1, 1]]` (`Moore`) or `[[0, 1, 0], [1, 0, 1], [0, 1, 0]]` (`VonNeumann`), which is somehow extremely fast with only one thread (props to Nil!) and then calculates the next field with functional-style maps and addition, multiplication, and comparisons.

## Licenses
For all licenses, look into `license.html`.  
This file was automatically created using [cargo-about](https://github.com/EmbarkStudios/cargo-about) (Embark Studios).