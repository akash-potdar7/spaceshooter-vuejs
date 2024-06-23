<template>
  <div class="enemyBullet" :style="{ left: bullet.x + 'px', top: bullet.y + 'px' }"></div>
</template>

<script>
  export default {
      props: ['bullet']
  };
</script>

<style>
  .enemyBullet {
      position: absolute;
      width: 5px;
      height: 10px;
      background: blue;
  }
</style>