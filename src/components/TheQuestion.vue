<template>
    <h1 class="question">{{ question }}</h1>
</template>

<script>
export default {
    name: 'TheQuestion',
    props: ['question']
}
</script>

<style scoped>
.question {
    animation-name: fadein;
    animation-duration: 2s;
    text-transform: uppercase;
    margin-left: 50px;
    margin-right: 50px;
    font-family: 'Work Sans', sans-serif;
}
</style>