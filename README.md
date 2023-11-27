<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Snake Game</title>
  <style>
    canvas {
      border: 1px solid #000;
      display: block;
      margin: 20px auto;
    }
  </style>
</head>
<body>
  <canvas id="snakeCanvas" width="400" height="400"></canvas>

  <script>
    const canvas = document.getElementById('snakeCanvas');
    const ctx = canvas.getContext('2d');

    const box = 20;
    let snake = [{ x: 10, y: 10 }];
    let food = { x: 15, y: 15 };
    let direction = 'right';

    function draw() {
      // Draw the snake
      ctx.fillStyle = 'green';
      snake.forEach(segment => ctx.fillRect(segment.x * box, segment.y * box, box, box));

      // Draw the food
      ctx.fillStyle = 'red';
      ctx.fillRect(food.x * box, food.y * box, box, box);
    }

    function move() {
      const head = { ...snake[0] };

      // Update the head position based on the direction
      if (direction === 'right') head.x++;
      else if (direction === 'left') head.x--;
      else if (direction === 'up') head.y--;
      else if (direction === 'down') head.y++;

      // Check for collisions with the walls or itself
      if (head.x < 0 || head.x >= canvas.width / box || head.y < 0 || head.y >= canvas.height / box || collision(head, snake)) {
        alert('Game Over!');
        snake = [{ x: 10, y: 10 }]; // Reset the snake
        direction = 'right';
      } else {
        snake.unshift(head); // Add the new head to the snake

        // Check if the snake eats the food
        if (head.x === food.x && head.y === food.y) {
          // Generate new food
          food = {
            x: Math.floor(Math.random() * (canvas.width / box)),
            y: Math.floor(Math.random() * (canvas.height / box))
          };
        } else {
          snake.pop(); // Remove the tail if no food is eaten
        }
      }
    }

    function collision(head, array) {
      // Check if the head collides with any segment of the snake
      return array.some(segment => segment.x === head.x && segment.y === head.y);
    }

    function gameLoop() {
      ctx.clearRect(0, 0, canvas.width, canvas.height); // Clear the canvas
      draw();
      move();
    }

    document.addEventListener('keydown', function (event) {
      // Update the direction based on arrow key input
      if (event.key === 'ArrowUp' && direction !== 'down') direction = 'up';
      else if (event.key === 'ArrowDown' && direction !== 'up') direction = 'down';
      else if (event.key === 'ArrowLeft' && direction !== 'right') direction = 'left';
      else if (event.key === 'ArrowRight' && direction !== 'left') direction = 'right';
    });

    setInterval(gameLoop, 100); // Run the game loop every 100 milliseconds
  </script>
</body>
</html>
