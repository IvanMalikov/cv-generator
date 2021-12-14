<template>
    <div class="avatar">
        <img :src="value">
    </div>
</template>

<script>
    export default {
        props: ['value']
    }

    //https://i.imgur.com/56Bu3l9.png
</script>

<style scoped>

    .avatar {
        display: flex;
        justify-content: center;
    }

    .avatar img {
        width: 150px;
        height: auto;
        border-radius: 50%;
        background-color: #111;
    }

</style>