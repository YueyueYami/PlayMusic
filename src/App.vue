<template>
    <div id="app">
        <div class="box">

        </div>
        <router-view />
    </div>
</template>

<style lang="scss">
    * {
        margin: 0;
        padding: 0;
        list-style: none;
    }

    body,
    html {
        height: 100vh,
    }

    #app {
        height: 100%;
        width: 100%;
        background: url('/public/1.jpg');
        background-position: center;
        background-attachment: fixed;
        position: relative;
        background-size: cover;
    }

    .box {
        position: absolute;
        left: 0;
        right: 0;
        width: 100%;
        height: calc(100vh - 100px);
        /* 主要内容 */
        background: rgba(0, 0, 0, 0.297);
        /* 模糊大小就是靠的blur这个函数中的数值大小 */
        backdrop-filter: blur(10px);
    }
</style>