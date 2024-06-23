<template>
  <div class="enemy" :style="{ left: enemy.x + 'px', top: enemy.y + 'px' }"></div>
</template>

<script>
export default {
  props: ['enemy'],
  mounted() {
    this.shootInterval = setInterval(() => {
      this.$emit('shoot', this.enemy.x + 25 - 2.5, this.enemy.y + 50);
    }, 2000); // Adjust timing for enemy shooting
  },
  beforeUnmount() {
    clearInterval(this.shootInterval);
  }
};
</script>

<style>
.enemy {
  position: absolute;
  width: 50px;
  height: 50px;
  background: url('../assets/evil.png') no-repeat center center / contain;
}
</style>