<script>
  import Background from './Background.svelte';
  import { onMount } from 'svelte';

  let text = 'I am into';
  let interests = ['Full-stack development', 'UX Design', 'Deep learning'];
  let currentInterest = '';
  let currentIndex = 0;
  let typingSpeed = 100;
  let deletingSpeed = 50;
  let families = [];
  let canvasWidth = window.innerWidth;
  let canvasHeight = window.innerHeight;
  let numNodesPerFamily = 10;
  let maxConnectionDistance = 125;
  let maxSpeed = 1;
  let cursorRadius = 100;

  onMount(() => {
    typeText();
    initializeFamilies();
    animate();
  });

  function typeText() {
    if (currentIndex < interests.length) {
      currentInterest = interests[currentIndex];
      currentIndex++;
      typeNextChar(0);
    } else {
      currentIndex = 0;
      setTimeout(typeText, 1000);
    }
  }

  function typeNextChar(index) {
    if (index <= currentInterest.length) {
      text = 'I am into ' + currentInterest.slice(0, index);
      setTimeout(() => typeNextChar(index + 1), typingSpeed);
    } else {
      setTimeout(deleteText, 1000);
    }
  }

  function deleteText() {
    deleteNextChar(currentInterest.length);
  }

  function deleteNextChar(index) {
    if (index >= 0) {
      text = 'I am into ' + currentInterest.slice(0, index);
      setTimeout(() => deleteNextChar(index - 1), deletingSpeed);
    } else {
      setTimeout(typeText, 1000);
    }
  }

  function initializeFamilies() {
    for (let i = 0; i < numNodesPerFamily; i++) {
      let family = [];
      for (let j = 0; j < numNodesPerFamily; j++) {
        family.push({
          x: Math.random() * canvasWidth,
          y: Math.random() * canvasHeight,
          color: getRandomColor(),
          size: Math.random() * 5 + 3,
          speedX: Math.random() * 2 - 1,
          speedY: Math.random() * 2 - 1,
        });
      }
      families.push(family);
    }

    for (let i = 0; i < numNodesPerFamily; i++) {
    let additionalFamily = [];
    for (let j = 0; j < numNodesPerFamily; j++) {
      additionalFamily.push({
        x: Math.random() * canvasWidth,
        y: Math.random() * canvasHeight,
        color: getRandomColor(),
        size: Math.random() * 5 + 3,
        speedX: Math.random() * 2 - 1,
        speedY: Math.random() * 2 - 1,
      });
    }
    families.push(additionalFamily);
    }
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
  families.forEach((familyA) => {
    familyA.forEach((nodeA) => {
      families.forEach((familyB) => {
        familyB.forEach((nodeB) => {
          if (nodeA !== nodeB) {
            const distance = getDistance(nodeA, nodeB);
            if (distance < maxConnectionDistance) {
              ctx.beginPath();
              ctx.moveTo(nodeA.x, nodeA.y);
              ctx.lineTo(nodeB.x, nodeB.y);
              ctx.strokeStyle = '#383a3c';
              ctx.stroke();
             }
            }
         });
       });
      });
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

    const distanceToCursor = getDistance(node, { x: cursorX, y: cursorY });
    if (distanceToCursor < cursorRadius) {
      const angle = Math.atan2(node.y - cursorY, node.x - cursorX);
      node.speedX += (cursorRadius - distanceToCursor) * Math.cos(angle) / cursorRadius;
      node.speedY += (cursorRadius - distanceToCursor) * Math.sin(angle) / cursorRadius;
    }

    const speed = Math.sqrt(node.speedX ** 2 + node.speedY ** 2);
    if (speed > maxSpeed) {
      const ratio = maxSpeed / speed;
      node.speedX *= ratio;
      node.speedY *= ratio;
    }
  }

  let cursorX = 0;
  let cursorY = 0;

  function updateCursor(event) {
    cursorX = event.clientX;
    cursorY = event.clientY;
  }

  function addNodesOnClick(event) {
    const x = event.clientX;
    const y = event.clientY;

    let newFamily = [];
    for (let j = 0; j < numNodesPerFamily; j++) {
      newFamily.push({
        x: x + Math.random() * 20 - 10,
        y: y + Math.random() * 20 - 10,
        color: getRandomColor(),
        size: Math.random() * 5 + 3,
        speedX: Math.random() * 2 - 1,
        speedY: Math.random() * 2 - 1,
      });
    }
    families.push(newFamily);
  }

  function getRandomColor() {
    return '#' + Math.floor(Math.random() * 16777215).toString(16);
  }

  window.addEventListener('click', addNodesOnClick);
  window.addEventListener('mousemove', updateCursor);
</script>

<style>
  main {
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    height: 100vh;
    text-align: center;
    position: relative;
    color: white;
  }

  .waving-emoji {
    border-radius: 10%;
    width: 5%;
    height: 5%;
    animation: wave 2s infinite;
    order: -1;
  }

  .greeting {
    margin-top: 5px;
    font-size: 24px;
  }

  .non-moving-picture {
    width: 300px;
    height: 320px;
    border-radius: 50%;
    margin-top: 5px;
  }

  @keyframes wave {
    100%, 75%, 50%, 25%, 0% {
      transform: rotate(0deg);
    }
    20%, 40%, 60%, 80% {
      transform: rotate(-30deg);
    }
  }
</style>

<main>
  <Background />
  <div class="greeting">
    <p>Hi<img class="waving-emoji" src="/images/waving-emoji.png" alt="Waving Emoji" />, I'm Martin Georgiev</p>
  </div>
  <img class="non-moving-picture" src="/images/logo.png" alt="Portrait Face" />
  <h1>{text}</h1>
  <canvas id="background-canvas" style="position: absolute; top: 0; left: 0;"></canvas>
</main>
