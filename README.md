```html
<!DOCTYPE html>
<html>
<head>
<title>Juego de Disparos</title>
<style>
body {
  margin: 0;
  overflow: hidden;
  background-color: #000; /* Fondo negro */
}

#canvas {
  background-color: #333; /* Fondo del lienzo */
}

#info {
  position: absolute;
  top: 10px;
  left: 10px;
  color: #fff;
  font-family: monospace;
}
</style>
</head>
<body>
<div id="info">Puntuación: <span id="score">0</span></div>
<canvas id="canvas"></canvas>

<script>
const canvas = document.getElementById('canvas');
const ctx = canvas.getContext('2d');
const scoreEl = document.getElementById('score');

// Ajustar el tamaño del canvas al tamaño de la ventana
canvas.width = window.innerWidth;
canvas.height = window.innerHeight;

// Variables del juego
let score = 0;
let player = {
  x: canvas.width / 2,
  y: canvas.height - 50,
  radius: 20,
  color: 'blue'
};
let projectiles = [];
let enemies = [];

// Función para crear enemigos
function createEnemy() {
  const radius = 15;
  const x = Math.random() * (canvas.width - radius * 2) + radius;
  const y = Math.random() * (canvas.height - radius * 2) + radius;
  enemies.push({ x, y, radius });
}

// Función para dibujar elementos
function draw() {
  ctx.clearRect(0, 0, canvas.width, canvas.height);

  // Dibujar al jugador
  ctx.beginPath();
  ctx.arc(player.x, player.y, player.radius, 0, Math.PI * 2);
  ctx.fillStyle = player.color;
  ctx.fill();

  // Dibujar proyectiles
  projectiles.forEach((projectile, index) => {
    ctx.beginPath();
    ctx.arc(projectile.x, projectile.y, projectile.radius, 0, Math.PI * 2);
    ctx.fillStyle = 'red';
    ctx.fill();

    // Mover proyectil
    projectile.y -= 5;

    // Eliminar proyectiles fuera de la pantalla
    if (projectile.y + projectile.radius < 0) {
      projectiles.splice(index, 1);
    }
  });

  // Dibujar enemigos
  enemies.forEach((enemy, index) => {
    ctx.beginPath();
    ctx.arc(enemy.x, enemy.y, enemy.radius, 0, Math.PI * 2);
    ctx.fillStyle = 'green';
    ctx.fill();
  });
}

// Función para actualizar el juego
function update() {
  // Crear enemigos aleatoriamente
  if (Math.random() < 0.01) {
    createEnemy();
  }

  // Comprobar colisiones entre proyectiles y enemigos
  projectiles.forEach((projectile, projectileIndex) => {
    enemies.forEach((enemy, enemyIndex) => {
      const distance = Math.hypot(projectile.x - enemy.x, projectile.y - enemy.y);
      if (distance < projectile.radius + enemy.radius) {
        // Eliminar enemigo y proyectil
        enemies.splice(enemyIndex, 1);
        projectiles.splice(projectileIndex, 1);

        // Aumentar la puntuación
        score++;
        scoreEl.textContent = score;
      }
    });
  });

  // Dibujar elementos
  draw();
}

// Capturar eventos del ratón
canvas.addEventListener('click', (event) => {
  const mouseX = event.clientX;
  const mouseY = event.clientY;

  // Crear un nuevo proyectil
  projectiles.push({
    x: player.x,
    y: player.y,
    radius: 5
  });
});

// Bucle de animación
setInterval(update, 10);
</script>
</body>
</html>
```
