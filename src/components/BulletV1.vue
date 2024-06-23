<template>
  <div class="bullet" :style="{ left: bullet.x + 'px', top: bullet.y + 'px' }"></div>
</template>

<script>
export default {
  props: ['bullet']
};
</script>

<style>
.bullet {
  position: absolute;
  width: 5px;
  height: 10px;
  background: red;
}
</style>