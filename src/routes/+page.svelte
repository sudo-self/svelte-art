<script>
  let drawing = false;
  let lastX = 0;
  let lastY = 0;
  let titleDisplayed = false;

  function handleMouseDown(event) {
    drawing = true;
    lastX = event.offsetX || event.touches[0].clientX;
    lastY = event.offsetY || event.touches[0].clientY;
    event.preventDefault(); // Prevent default behavior
  }

  function handleMouseUp() {
    drawing = false;
  }

  function handleMouseMove(event) {
    if (!drawing) return;

    const canvas = document.getElementById('canvas');
    const ctx = canvas.getContext('2d');

    const mouseX = event.offsetX || event.touches[0].clientX;
    const mouseY = event.offsetY || event.touches[0].clientY;

    const brushSize = document.getElementById('brushSize').value;
    const isEraser = document.getElementById('eraser').checked;

    ctx.beginPath();
    ctx.moveTo(lastX, lastY);
    ctx.lineTo(mouseX, mouseY);
    ctx.lineWidth = brushSize;

    if (isEraser) {
      ctx.strokeStyle = 'white'; // Set color to white for eraser
    } else {
      ctx.strokeStyle = document.getElementById('colorPicker').value;
    }

    ctx.lineCap = 'round'; // Make the brush stroke rounded
    ctx.stroke();

    lastX = mouseX;
    lastY = mouseY;
    event.preventDefault(); // Prevent default behavior
  }

  function clearCanvas() {
    const canvas = document.getElementById('canvas');
    const ctx = canvas.getContext('2d');

    ctx.clearRect(0, 0, canvas.width, canvas.height);
    titleDisplayed = false;
  }

  function clearTitle() {
    const canvas = document.getElementById('canvas');
    const ctx = canvas.getContext('2d');

    if (titleDisplayed) {
      ctx.clearRect(0, 0, canvas.width, 50);
      titleDisplayed = false;
    }
  }

  function drawTitle(event) {
    const canvas = document.getElementById('canvas');
    const ctx = canvas.getContext('2d');

    if (event.key === 'Enter') {
      const title = event.target.value;
      const titleX = canvas.width / 2;
      const titleY = 30;

      ctx.font = '24px Arial';
      ctx.fillStyle = document.getElementById('titleColorPicker').value; // Set title color
      ctx.textAlign = 'center';
      ctx.fillText(title, titleX, titleY); // Display title at the top of the canvas
      event.target.value = ''; // Clear input after displaying title
      titleDisplayed = true;
    }
  }

  function shareCanvas() {
    const canvas = document.getElementById('canvas');
    const dataUrl = canvas.toDataURL();
    const link = document.createElement('a');
    link.href = dataUrl;
    link.download = 'svelteart.png'; 
    link.click();
  }
</script>

<style>
  main {
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    height: 100vh;
    overflow: hidden; /* Disable scrolling */
  }

  canvas {
    border: 1px solid #000;
    cursor: crosshair;
  }

  label {
    margin-right: 5px;
  }

  footer {
    text-align: center;
    margin-top: 20px;
  }
</style>

<main>
  <h1>create the day...</h1>
  <canvas id="canvas" width="400" height="500" style="margin: 20px;" on:mousedown="{handleMouseDown}" on:mouseup="{handleMouseUp}" on:mousemove="{handleMouseMove}" on:touchstart="{handleMouseDown}" on:touchend="{handleMouseUp}" on:touchmove="{handleMouseMove}"></canvas>
  <div>
    <label for="colorPicker">Color:</label>
    <input type="color" id="colorPicker" value="#000000">
    <label for="brushSize" style="margin-left: 10px;">Brush Size:</label>
    <input type="number" id="brushSize" min="1" max="20" value="2">
    <label for="eraser" style="margin-left: 10px;">Eraser:</label>
    <input type="checkbox" id="eraser">
    <button on:click="{clearCanvas}" style="margin-left: 10px;">Clear</button>
    <button on:click="{shareCanvas}" style="margin-left: 10px;">Save</button>
  </div>
  <div>
    <label for="titleInput">Title:</label>
    <input type="text" id="titleInput" on:keydown="{drawTitle}">
    <label for="titleColorPicker" style="margin-left: 10px;">Title Color:</label>
    <input type="color" id="titleColorPicker" value="#000000">
    <button on:click="{clearTitle}" style="margin-left: 10px;">Clear Title</button>
  </div>
  <footer> <img src="https://api.iconify.design/logos:svelte.svg?color=%234d22b3" alt="Svelte Icon" style="width: 24px; height: 24px; margin-left: 10px;">Art by sudo-self</footer>
</main>
