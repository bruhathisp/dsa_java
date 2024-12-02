# Code to run 

[Link](https://algorithm-visualizer.org)


``` javascript
// Import visualization libraries
const { Tracer, Array2DTracer, LogTracer, Layout, VerticalLayout } = require('algorithm-visualizer');

// Input matrix
const matrix = [
  [1, 2, 3],
  [4, 5, 6],
  [7, 8, 9]
];

// Define tracer variables
const matrixTracer = new Array2DTracer('Matrix');
const logger = new LogTracer();
Layout.setRoot(new VerticalLayout([matrixTracer, logger]));

// Set and visualize the initial matrix
matrixTracer.set(matrix);
logger.println('Initial matrix:');
Tracer.delay();

// Function to rotate the matrix
function rotateMatrix(matrix) {
  const n = matrix.length;

  logger.println('Starting matrix rotation...');
  Tracer.delay();

  // Transpose the matrix
  logger.println('Transposing the matrix...');
  for (let i = 0; i < n; i++) {
    for (let j = i; j < n; j++) {
      logger.println(`Swapping elements [${i}][${j}] and [${j}][${i}]`);
      // Visualize swap
      matrixTracer.select(i, j);
      matrixTracer.select(j, i);
      Tracer.delay();

      const temp = matrix[i][j];
      matrix[i][j] = matrix[j][i];
      matrix[j][i] = temp;

      // Update visualization
      matrixTracer.patch(i, j, matrix[i][j]);
      matrixTracer.patch(j, i, matrix[j][i]);
      Tracer.delay();

      matrixTracer.depatch(i, j);
      matrixTracer.depatch(j, i);
      matrixTracer.deselect(i, j);
      matrixTracer.deselect(j, i);
    }
  }

  // Reverse each row
  logger.println('Reversing each row...');
  for (let i = 0; i < n; i++) {
    for (let j = 0; j < Math.floor(n / 2); j++) {
      logger.println(`Swapping row ${i} elements [${j}] and [${n - j - 1}]`);
      // Visualize swap
      matrixTracer.select(i, j);
      matrixTracer.select(i, n - j - 1);
      Tracer.delay();

      const temp = matrix[i][j];
      matrix[i][j] = matrix[i][n - j - 1];
      matrix[i][n - j - 1] = temp;

      // Update visualization
      matrixTracer.patch(i, j, matrix[i][j]);
      matrixTracer.patch(i, n - j - 1, matrix[i][n - j - 1]);
      Tracer.delay();

      matrixTracer.depatch(i, j);
      matrixTracer.depatch(i, n - j - 1);
      matrixTracer.deselect(i, j);
      matrixTracer.deselect(i, n - j - 1);
    }
  }

  logger.println('Matrix rotation complete.');
  Tracer.delay();
}

// Execute the function
rotateMatrix(matrix);
matrixTracer.set(matrix);
logger.println('Rotated matrix:');

```
