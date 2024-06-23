<template>
  <div id="player" :style="{ left: x + 'px', bottom: '20px' }"></div>
</template>

<script>
export default {
  data() {
    return {
      x: window.innerWidth / 2 - 25,
      keys: {}
    };
  },
  methods: {
    handleKeyDown(e) {
      this.keys[e.code] = true;
    },
    handleKeyUp(e) {
      this.keys[e.code] = false;
    },
    update() {
      if (this.keys['ArrowLeft'] && this.x > 0) {
        this.x -= 5;
      }
      if (this.keys['ArrowRight'] && this.x < window.innerWidth - 50) {
        this.x += 5;
      }
      if (this.keys['Space']) {
        this.$emit('shoot', this.x + 22.5, 20); // Emit shoot event with correct coordinates
      }
    },
    getRect() {
      return this.$el.getBoundingClientRect();
    }
  },
  mounted() {
    setInterval(this.update, 1000 / 60); // Example update interval
    window.addEventListener('keydown', this.handleKeyDown);
    window.addEventListener('keyup', this.handleKeyUp);
  },
  beforeUnmount() {
    window.removeEventListener('keydown', this.handleKeyDown);
    window.removeEventListener('keyup', this.handleKeyUp);
  }
};
</script>

<style>
#player {
  position: absolute;
  width: 50px;
  height: 50px;
  background: url('../assets/ship_0.png') no-repeat center center / contain;
}
</style>