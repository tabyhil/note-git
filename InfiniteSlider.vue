<template>
    <div class="container">
        <ul class="wrap">
            <li class="card" v-for="(item, index) in  testImages ">
                <img :src="item" alt="">
            </li>
            <li class="card" v-for="( item, index ) in  testImages ">
                <img :src="item" alt="">
            </li>
        </ul>
    </div>
</template>

<script setup>
import { ref } from 'vue';
/* 使用规范 */
/* let basicSetting = {
    container: {
        width: 1000,
        height: 500
    },
    card: {
        width: 300,
        height: 300
    },
    num: 4
} */

let testImages = ref(["/public/imgs/home/bg-1.jpg", "/public/imgs/home/bg-2.jpeg", "/public/imgs/home/bg-3.jpg", "/public/imgs/home/bg-4.jpg"]);
console.log(testImages.value);
let { container, card, num } = defineProps({
    container: {
        width: Number,
        height: Number
    },
    card: {
        width: Number,
        height: Number
    },
    num: Number
})
//wrap最佳间距
let bestMargin = (container.width - num * card.width) / num;
console.log(bestMargin);
// wrap最佳长度
let bestWrapWidth = bestMargin > 0 ? parseInt((2 * num - 1) * bestMargin + (2 * num) * card.width) : card.width * num * 2;
//最佳滑动距离
let slideDistance = bestMargin > 0 ? container.width : bestWrapWidth / 2;
</script>

<style scoped>
.container {
    width: v-bind('container.width + "px"');
    height: v-bind('container.height + "px"');
    background-color: black;
    margin: 0 auto;
    overflow: hidden;
}


.wrap {
    transform: translate(0px, 0px);
    width: v-bind('bestWrapWidth + "px"');
    height: v-bind('container.height + "px"');
    display: inline-flex;
    justify-content: space-between;
    align-items: center;
    animation: 3s linear infinite slide;
}

.card {
    width: v-bind('card.width + "px"');
    height: v-bind('card.height + "px"');
    background-color: green;
}

@keyframes slide {
    from {}

    to {
        transform: translate(v-bind('"-" + slideDistance + "px"'), 0px);
    }
}

img {
    width: v-bind('card.width + "px"');
    height: v-bind('card.height + "px"');
}
</style>