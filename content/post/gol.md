+++
title = "Conways Game of Life Simulation"
date = "2022-03-07"
cover = "img/1200x600_Life.jpg"
description = "Implementation of Game of Life Simulation using Golang for my Computer Systems A module at the University of Bristol"
contentTypeName = "projects"
+++

### What is Life?

The Game of Life is a cellular automaton devised by the British mathematician John Horton Conway in 1970. It is somehow a game, where you can only get to set the starting life/board of the game. I personally prefer calling this a simulation for this reason. 

This simulation runs on a (infinite) 2D grid of cells where the life of the next generation is determined by applying a set of rules on the current generation.

&nbsp;

### The rules are best summarised as:

1. Any live cell with two or three live neighbours survives.

2. Any dead cell with three live neighbours becomes a live cell.

3. All other live cells die in the next generation. Similarly, all other dead cells stay dead.

&nbsp;

This ruleset for the next generation is implement by the function

```go
// golang
func calculateNextState(p Params, world [][]byte) [][]byte {

	newWorld := make([][]byte, p.ImageHeight)
	for i := range newWorld {
		newWorld[i] = make([]byte, p.ImageWidth)
	}
	for col := 0; col < p.ImageHeight; col++ {
		for row := 0; row < p.ImageWidth; row++ {
			n := neighbours(p.ImageWidth, col , row , world)
			if world[col][row] == 255 {
				if n == 2 || n == 3 {
					newWorld[col][row] = 255
				}
			} else {
				if n == 3 {
					newWorld[col][row] = 255
				}
			}
		}
	}
	return newWorld
}
```
&nbsp;

Where the neighbours function return the number of alive cells/pixels directly adjacent to it
&nbsp;

```go
func neighbours(width, y, x int, world [][]uint8) int {
	height := len(world)
	neighbours := 0
	for i := -1; i <= 1; i++ {
		for j := -1; j <= 1; j++ {
			if i != 0 || j != 0 {
				h := (y + height + i) % height
				w := (x + width + j) % width
				if world[h][w] == 255 {
					neighbours++
				}
			}
	return neighbours
}
```

&nbsp;

The resulting simulation looks like this 

![GOL Simulation](/img/gol_new.gif)

&nbsp;

This set of rules may seem very simple and mundane, however it brings life to very many cool and interesting patterns. 

One of my favourite patterns is this little guy, who just travels across the board. This simulation is called a glider.

![Glider](/img/gol_glider.gif)

&nbsp;

### Further Optimizations and Approaches
---------------------------------------

1) Parallelising.

As you may have thought, doing the simulation over a bigger area would become slow and unusable as we have one thread working to compute everything. 

We can use multiple threads to parrallelize and make the simulation compute faster. 

We would just need to split the world and pass it onto different threads to compute the result quicker.

The implementation for this can be found [here](https://github.com/nafaaal/gol-coursework)

---------------------------------------

2) Distribution.

For some weird reason we might not want to compute this locally, and in that case we can use remote procedure calls to servers to do the hard work for us and give us back the result. 

This distributed method can also scale like the other parallel method, by sending computation over to multiple servers. 

However the network latency to send and recieve large arrays seems to be the bigger bottleneck in this approach

The code for this implementation can be found [here](https://github.com/nafaaal/distributed-gol-coursework)

---------------------------------------


&nbsp;

> You may have wondered what the banner image was about. Sorry about the quality, it was my 2 minute attempt of writing "Life" using Conways Script, which is an alphabet inspired by this simulation. More information about it linked [here](https://omniglot.com/conscripts/conway.htm)
