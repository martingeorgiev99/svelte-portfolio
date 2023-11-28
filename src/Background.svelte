<script>
  import { onMount } from 'svelte';

  let families = [];

  const canvasWidth = window.innerWidth;
  const canvasHeight = window.innerHeight;
  const numFamilies = 5;
  const numNodesPerFamily = 15;
  const maxConnectionDistance = 200;

  onMount(() => {
    initializeFamilies();
    animate();
  });

  function initializeFamilies() {
    for (let i = 0; i < numFamilies; i++) {
      let family = [];
      for (let j = 0; j < numNodesPerFamily; j++) {
        family.push({
          x: Math.random() * canvasWidth,
          y: Math.random() * canvasHeight,
          color: getRandomColor(),
          size: Math.random() * 10 + 5,
          speedX: Math.random() * 2 - 1,
          speedY: Math.random() * 2 - 1,
        });
      }
      families.push(family);
    }
  }

  function getRandomColor() {
    return '#' + Math.floor(Math.random() * 16777215).toString(16);
  }

  function animate() {
    requestAnimationFrame(animate);

    const canvas = document.getElementById('background-canvas');
    const ctx = canvas.getContext('2d');
    ctx.clearRect(0, 0, canvasWidth, canvasHeight);
    ctx.fillStyle = '#141516';
    ctx.fillRect(0, 0, canvasWidth, canvasHeight);

    drawLines(ctx);
    drawNodes(ctx);
  }

  function drawLines(ctx) {
    ctx.strokeStyle = '#999';

    families.forEach((family) => {
      for (let i = 0; i < family.length; i++) {
        for (let j = i + 1; j < family.length; j++) {
          const distance = getDistance(family[i], family[j]);
          if (distance < maxConnectionDistance) {
            ctx.beginPath();
            ctx.moveTo(family[i].x, family[i].y);
            ctx.lineTo(family[j].x, family[j].y);
            ctx.stroke();
          }
        }
      }
    });
  }

  function getDistance(node1, node2) {
    const dx = node1.x - node2.x;
    const dy = node1.y - node2.y;
    return Math.sqrt(dx * dx + dy * dy);
  }

  function drawNodes(ctx) {
    families.forEach((family) => {
      family.forEach((node) => {
        moveNode(node);
        ctx.fillStyle = node.color;
        ctx.beginPath();
        ctx.arc(node.x, node.y, node.size, 0, 2 * Math.PI);
        ctx.fill();
      });
    });
  }

  function moveNode(node) {
    node.x += node.speedX;
    node.y += node.speedY;


    if (node.x < 0 || node.x > canvasWidth) {
      node.speedX *= -1;
    }

    if (node.y < 0 || node.y > canvasHeight) {
      node.speedY *= -1;
    }
  }
</script>

<style>
  canvas {
    position: fixed;
    top: 0;
    left: 0;
    z-index: -1;
  }
</style>

<canvas id="background-canvas" width={canvasWidth} height={canvasHeight}></canvas>
