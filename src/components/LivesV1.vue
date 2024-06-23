<template>
    <div id="lives">Lives: {{ lives }}</div>
</template>

<script>
    export default {
        props: ['lives']
    };
</script>

<style>
    #lives {
        position: absolute;
        top: 10px;
        right: 10px;
        color: white;
    }
</style>