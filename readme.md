# Animated Wall Clock

## Overview

This project is an animated wall clock implemented using HTML, CSS, and JavaScript. The clock allows customization of its appearance and provides functionality to save the current clock view as an image.

## Features

- **Real-time Animated Clock**: Displays the current time with smooth animations for the hour, minute, and second hands.
- **Customizable Appearance**: Users can change the colors of the clock's face, border, number lines, and hands.
- **Save as Image**: Allows saving the current state of the clock as a PNG image.

## Getting Started

### Prerequisites

- A modern web browser (Chrome, Firefox, Safari, etc.)
- Basic knowledge of HTML, CSS, and JavaScript

### Installation

1. Clone the repository:
   ```bash
   git clone https://github.com/HopSoft-Tech/animated-wall-clock.git
   ```
2. Navigate to the project directory:
   ```bash
   cd animated-wall-clock
   ```
3. Open `index.html` in your web browser to view the clock.

### Usage

1. Open `index.html` in your web browser.
2. Use the form to customize the clock's appearance:
   - **Face Color**: Change the color of the clock's face.
   - **Border Color**: Change the color of the clock's border.
   - **Number Lines Color**: Change the color of the hour and minute lines.
   - **Large Hands Color**: Change the color of the hour and minute hands.
   - **Second Hand Color**: Change the color of the second hand.
3. Click the "Save As Image" button to save the current state of the clock as a PNG file.

## Code Snippets

### HTML Structure

The basic structure of the HTML file includes a form for customizing the clock and a canvas for drawing the clock.

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <link rel="stylesheet" href="style.css" />
    <script src="app.js" defer></script>
    <title>Animated Clock</title>
</head>
<body>
    <h1>Animated Clock</h1>
    <div class="card">
        <form>
            <div class="form-input">
                <label for="face-color">Face Color</label>
                <input type="color" value="#f4f4f4" id="face-color" />
            </div>
            <div class="form-input">
                <label for="border-color">Border Color</label>
                <input type="color" value="#800000" id="border-color" />
            </div>
            <div class="form-input">
                <label for="line-color">Number Lines Color</label>
                <input type="color" value="#000000" id="line-color" />
            </div>
            <div class="form-input">
                <label for="large-hand-color">Large Hands Color</label>
                <input type="color" value="#800000" id="large-hand-color" />
            </div>
            <div class="form-input">
                <label for="second-hand-color">Second Hand Color</label>
                <input type="color" value="#FF7F50" id="second-hand-color" />
            </div>
            <div class="form-input">
                <button id="save-btn" type="button">Save As Image</button>
            </div>
        </form>
    </div>
    <div id="canvas-container">
        <canvas id="canvas" width="500" height="500"></canvas>
    </div>
</body>
</html>
```

### JavaScript Functionality

The JavaScript code handles the drawing of the clock and the customization logic.

```javascript
const canvas = document.getElementById("canvas");
const faceColor = document.getElementById("face-color");
const borderColor = document.getElementById("border-color");
const lineColor = document.getElementById("line-color");
const largeHandColor = document.getElementById("large-hand-color");
const secondHandColor = document.getElementById("second-hand-color");

function clock() {
  const now = new Date();
  const ctx = canvas.getContext("2d");

  ctx.save();
  ctx.clearRect(0, 0, 500, 500);
  ctx.translate(250, 250);
  ctx.rotate(-Math.PI / 2);

  // Draw clock face
  ctx.beginPath();
  ctx.lineWidth = 14;
  ctx.strokeStyle = borderColor.value;
  ctx.fillStyle = faceColor.value;
  ctx.arc(0, 0, 142, 0, Math.PI * 2, true);
  ctx.stroke();
  ctx.fill();
  ctx.restore();

  // Draw hour lines
  ctx.save();
  ctx.strokeStyle = lineColor.value;
  for (let i = 0; i < 12; i++) {
    ctx.beginPath();
    ctx.rotate(Math.PI / 6);
    ctx.moveTo(100, 0);
    ctx.lineTo(120, 0);
    ctx.stroke();
  }
  ctx.restore();

  // Draw minute lines
  ctx.save();
  ctx.strokeStyle = lineColor.value;
  ctx.lineWidth = 4;
  for (let i = 0; i < 60; i++) {
    if (i % 5 !== 0) {
      ctx.beginPath();
      ctx.moveTo(117, 0);
      ctx.lineTo(120, 0);
      ctx.stroke();
    }
    ctx.rotate(Math.PI / 30);
  }
  ctx.restore();

  // Draw hour hand
  const hr = now.getHours() % 12;
  const min = now.getMinutes();
  const sec = now.getSeconds();
  ctx.save();
  ctx.rotate((Math.PI / 6) * hr + (Math.PI / 360) * min + (Math.PI / 21600) * sec);
  ctx.strokeStyle = largeHandColor.value;
  ctx.lineWidth = 14;
  ctx.beginPath();
  ctx.moveTo(-20, 0);
  ctx.lineTo(80, 0);
  ctx.stroke();
  ctx.restore();

  // Draw minute hand
  ctx.save();
  ctx.rotate((Math.PI / 30) * min + (Math.PI / 1800) * sec);
  ctx.strokeStyle = largeHandColor.value;
  ctx.lineWidth = 10;
  ctx.beginPath();
  ctx.moveTo(-28, 0);
  ctx.lineTo(112, 0);
  ctx.stroke();
  ctx.restore();

  // Draw second hand
  ctx.save();
  ctx.rotate((sec * Math.PI) / 30);
  ctx.strokeStyle = secondHandColor.value;
  ctx.lineWidth = 6;
  ctx.beginPath();
  ctx.moveTo(-30, 0);
  ctx.lineTo(100, 0);
  ctx.stroke();
  ctx.restore();

  ctx.restore();

  requestAnimationFrame(clock);
}

requestAnimationFrame(clock);

document.getElementById("save-btn").addEventListener("click", () => {
  const dataURL = canvas.toDataURL("image/png");
  const link = document.createElement("a");
  link.download = "clock.png";
  link.href = dataURL;
  link.click();
});
```

### CSS Styling

The CSS file styles the form and the clock canvas.

```css
body {
  font-family: Arial, sans-serif;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  height: 100vh;
  margin: 0;
  background-color: #f0f0f0;
}

.card {
  background-color: #ffffff;
  border-radius: 8px;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
  padding: 20px;
  margin-bottom: 20px;
}

.form-input {
  margin-bottom: 10px;
}

#canvas-container {
  display: flex;
  justify-content: center;
}
```

## License

This project is licensed under the MIT License.

