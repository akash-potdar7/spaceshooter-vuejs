<template>
  <div id="gameArea">
    <div class="background"></div>
    <Player ref="player" />
    <Score :score="score" />
    <Lives :lives="lives" />
    <Bullet v-for="bullet in bullets" :key="bullet.id" :bullet="bullet" />
    <Enemy v-for="enemy in enemies" :key="enemy.id" :enemy="enemy" @shoot="createEnemyBullet" />
    <EnemyBullet v-for="bullet in enemyBullets" :key="bullet.id" :bullet="bullet" />
    <Asteroid v-for="asteroid in asteroids" :key="asteroid.id" :asteroid="asteroid" />
  </div>
</template>

<script>
import Player from './PlayerV1.vue';
import Score from './ScoreV1.vue';
import Lives from './LivesV1.vue';
import Bullet from './BulletV1.vue';
import Enemy from './EnemyV1.vue';
import EnemyBullet from './EnemyBulletV1.vue';
import Asteroid from './AsteroidV1.vue';

export default {
  components: {
    Player,
    Score,
    Lives,
    Bullet,
    Enemy,
    EnemyBullet,
    Asteroid
  },
  data() {
    return {
      score: 0,
      lives: 3,
      bullets: [],
      enemies: [],
      enemyBullets: [],
      asteroids: []
    };
  },
  mounted() {
    this.spawnObjects();
    this.increaseDifficulty();
    this.update();
    window.addEventListener('keydown', this.handleKeyDown);
    window.addEventListener('keyup', this.handleKeyUp);
  },
  beforeUnmount() {
    window.removeEventListener('keydown', this.handleKeyDown);
    window.removeEventListener('keyup', this.handleKeyUp);
  },
  methods: {
    handleKeyDown(e) {
      this.$refs.player.handleKeyDown(e);
    },
    handleKeyUp(e) {
      this.$refs.player.handleKeyUp(e);
    },
    createBullet(x, y) {
      const id = Date.now() + Math.random();
      this.bullets.push({ id, x, y });
    },
    createEnemy() {
      const id = Date.now() + Math.random();
      const x = Math.random() * (window.innerWidth - 50);
      this.enemies.push({ id, x, y: 0, speed: 2 });
    },
    createAsteroid() {
      const id = Date.now() + Math.random();
      const x = Math.random() * (window.innerWidth - 50);
      this.asteroids.push({ id, x, y: 0, speed: 1 });
    },
    createEnemyBullet(x, y) {
      const id = Date.now() + Math.random();
      this.enemyBullets.push({ id, x, y });
    },
    spawnObjects() {
      setInterval(this.createEnemy, 1000);
      setInterval(this.createAsteroid, 1500);
    },
    updateScore() {
      // Logic to update score
    },
    updateLives() {
      // Logic to update lives
    },
    increaseDifficulty() {
      setInterval(() => {
        this.enemies.forEach(enemy => {
          enemy.speed += 0.5;
        });
        this.asteroids.forEach(asteroid => {
          asteroid.speed += 0.5;
        });
      }, 10000);
    },
    checkCollision(rect1, rect2) {
      return !(
        rect1.right < rect2.left ||
        rect1.left > rect2.right ||
        rect1.bottom < rect2.top ||
        rect1.top > rect2.bottom
      );
    },
    update() {
      this.$refs.player.update();
      this.bullets.forEach((bullet, index) => {
        bullet.y -= 7;
        if (bullet.y + 10 < 0) {
          this.bullets.splice(index, 1);
        }
      });
      this.enemies.forEach((enemy, enemyIndex) => {
        enemy.y += enemy.speed;
        if (enemy.y > window.innerHeight) {
          this.enemies.splice(enemyIndex, 1);
        }
        // Enemy shooting logic
        if (Math.random() < 0.02) {
          this.createEnemyBullet(enemy.x + 25 - 2.5, enemy.y + 50);
        }
        this.enemyBullets.forEach((bullet, bulletIndex) => {
          bullet.y += 5;
          if (bullet.y > window.innerHeight) {
            this.enemyBullets.splice(bulletIndex, 1);
          }
          if (this.checkCollision(bullet, this.$refs.player.getRect())) {
            this.enemyBullets.splice(bulletIndex, 1);
            this.lives--;
            this.updateLives();
            if (this.lives <= 0) {
              alert('Game Over');
              window.location.reload();
            }
          }
        });
        this.bullets.forEach((bullet, bulletIndex) => {
          if (this.checkCollision(bullet, enemy)) {
            this.bullets.splice(bulletIndex, 1);
            this.enemies.splice(enemyIndex, 1);
            this.score += 10;
            this.updateScore();
          }
        });
        if (this.checkCollision(this.$refs.player.getRect(), enemy)) {
          this.enemies.splice(enemyIndex, 1);
          this.lives--;
          this.updateLives();
          if (this.lives <= 0) {
            alert('Game Over');
            window.location.reload();
          }
        }
      });
      this.asteroids.forEach((asteroid, asteroidIndex) => {
        asteroid.y += asteroid.speed;
        if (asteroid.y > window.innerHeight) {
          this.asteroids.splice(asteroidIndex, 1);
        }
        this.bullets.forEach((bullet, bulletIndex) => {
          if (this.checkCollision(bullet, asteroid)) {
            this.bullets.splice(bulletIndex, 1);
            this.asteroids.splice(asteroidIndex, 1);
            this.score += 5;
            this.updateScore();
          }
        });
        if (this.checkCollision(this.$refs.player.getRect(), asteroid)) {
          this.asteroids.splice(asteroidIndex, 1);
          this.lives--;
          this.updateLives();
          if (this.lives <= 0) {
            alert('Game Over');
            window.location.reload();
          }
        }
      });
      requestAnimationFrame(this.update);
    }
  }
};
</script>

<style>
  #gameArea {
    position: relative;
    width: 100%;
    height: 100vh;
    overflow: hidden;
  }

  .background {
    position: absolute;
    width: 100%;
    height: 200%;
    background-size: cover;
    top: -100%;
  }
</style>