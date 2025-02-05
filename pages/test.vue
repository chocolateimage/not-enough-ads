<template>
    <div class="wrapper">
        <AdView v-for="i in 50" :key="i" class="ad" />
    </div>
</template>
<style scoped>
.wrapper {
    display: flex;
    justify-content: center;
    align-items: center;
    flex-wrap: wrap;

    width: 100vw;
    height: 100vh;
}

@keyframes adAnimation {
    0% {
        width: 300px;
        height: 800px;
    }
    50% {
        width: 800px;
        height: 300px;
    }
    100% {
        width: 300px;
        height: 800px;
    }
}

.ad {
    width: 500px;
    height: 500px;

    animation: adAnimation 10s ease-in-out infinite;
}
</style>