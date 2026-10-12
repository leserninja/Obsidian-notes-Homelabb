
#### javascript

**Arrays**

A grid layout is made somewhat like this: 


## my verision
```
var cols = 40;
var rows = 40;


function setup() {
  createCanvas(400, 400);

}


function draw() {
  background(220);
  frameRate(1);

  for (var i = 0; i < cols ; i++) {
      for (var j = 0; j < cols ; j++) {
        var x = i * 30
        var y = j * 30
        stroke(0);
        fill(255);
        rect(x ,y, 30, 30);
      }
    
  }
}
```


## full shorted verision

const res = 10;
let cols, rows, grid;

function setup() {
  createCanvas(600, 400);
  cols = width / res;
  rows = height / res;
  // Create a 2D grid filled randomly with 0 (dead) or 1 (alive)
  grid = Array.from({ length: cols }, () =>
    Array.from({ length: rows }, () => floor(random(2)))
  );
}

function draw() {
  background(255);
  fill(0);

  // Grid to store the next generation frame
  let next = Array.from({ length: cols }, () => new Array(rows));

  for (let i = 0; i < cols; i++) {
    for (let j = 0; j < rows; j++) {
      // 1. Draw live cells
      if (grid[i][j] === 1) {
        rect(i * res, j * res, res - 1, res - 1);
      }

      // 2. Count neighbors and calculate the next state
      let neighbors = countNeighbors(i, j);
      let state = grid[i][j];

      if (neighbors === 3) next[i][j] = 1;        // Birth or survive
      else if (neighbors === 2) next[i][j] = state; // Stay the same
      else next[i][j] = 0;                        // Die (under/overpopulation)
    }
  }

  grid = next;
}

function countNeighbors(x, y) {
  let sum = 0;
  for (let i = -1; i <= 1; i++) {
    for (let j = -1; j <= 1; j++) {
      let col = (x + i + cols) % cols;
      let row = (y + j + rows) % rows;
      sum += grid[col][row];
    }
  }
  return sum - grid[x][y]; // Subtract the cell itself
}


## verision original




function make2DArray(cols, rows) {
  let arr = new Array(cols);
  for (let i = 0; i < arr.length; i++) {
    arr[i] = new Array(rows);
  }
  return arr;
}

let grid;
let cols;
let rows;
let resolution = 10;

function setup() {
  createCanvas(600, 400);
  cols = width / resolution;
  rows = height / resolution;

  grid = make2DArray(cols, rows);
  for (let i = 0; i < cols; i++) {
    for (let j = 0; j < rows; j++) {
      grid[i][j] = floor(random(2));
    }
  }
}

function draw() {
  background(255);

  for (let i = 0; i < cols; i++) {
    for (let j = 0; j < rows; j++) {
      let x = i * resolution;
      let y = j * resolution;
      if (grid[i][j] == 1) {
        fill(0);
        stroke(0);
        rect(x, y, resolution - 1, resolution - 1);
      }
    }
  }

  let next = make2DArray(cols, rows);

  // Compute next based on grid
  for (let i = 0; i < cols; i++) {
    for (let j = 0; j < rows; j++) {
      let state = grid[i][j];
      // Count live neighbors!
      let sum = 0;
      let neighbors = countNeighbors(grid, i, j);

      if (state == 0 && neighbors == 3) {
        next[i][j] = 1;
      } else if (state == 1 && (neighbors < 2 || neighbors > 3)) {
        next[i][j] = 0;
      } else {
        next[i][j] = state;
      }

    }
  }

  grid = next;



}


function countNeighbors(grid, x, y) {
  let sum = 0;
  for (let i = -1; i < 2; i++) {
    for (let j = -1; j < 2; j++) {
      let col = (x + i + cols) % cols;
      let row = (y + j + rows) % rows;
      sum += grid[col][row];
    }
  }
  sum -= grid[x][y];
  return sum;
}