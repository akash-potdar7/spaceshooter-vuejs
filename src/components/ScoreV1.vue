<template>
    <div id="score">Score: {{ score }}</div>
</template>

<script>
export default {
    props: ['score']
};
</script>

<style>
#score {
    position: absolute;
    top: 10px;
    left: 10px;
    color: white;
}
</style>