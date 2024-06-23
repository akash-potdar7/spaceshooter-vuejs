<template>
    <div class="asteroid" :style="{ left: asteroid.x + 'px', top: asteroid.y + 'px' }"></div>
</template>

<script>
export default {
    props: ['asteroid']
};
</script>

<style>
    .asteroid {
        position: absolute;
        width: 50px;
        height: 50px;
        background: url('../assets/asteroid_small_0.png') no-repeat center center / contain;
    }
</style>