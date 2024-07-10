<template>
    <span class="my-icon"></span>
</template>

<script>

</script>

<style scoped>
.my-icon {
    position: absolute;
    display: block;
    z-index: 2;
    left: 10px;
    top: 9px;
    width: 18px;
    height: 18px;
}

.my-icon--volume-on {
    background-image: url("../assets/volume-high.svg");
}

.my-icon--volume-off {
    background-image: url("../assets/volume-off.svg");
}
</style>