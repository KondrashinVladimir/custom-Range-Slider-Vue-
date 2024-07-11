<template>
    <button type="button">
        <slot></slot>
    </button>
</template>

<script>

</script>

<style scoped>
.my-button {
    box-sizing: border-box;
    position: relative;
    background-color: var(--colorAccent);
    margin: 0;
    padding: 0;
    width: 40px;
    height: 40px;
    border: 1px solid rgba(255, 110, 64, 0);
    border-radius: 50%;
    text-align: center;
    display: block;
    cursor: pointer;
    transition: all 0.2s ease-out;

    &:hover {
        background-color: rgba(255, 110, 64, 0.1);
    }

    &:focus-visible {
        outline: none;
        border: 1px solid var(--colorPrimaryHint);
        background-color: rgba(255, 110, 64, 0.1); 
    }

    &:active {
        background-color: rgba(255, 110, 64, 0.3);
    }
}

.my-button--volume-on::after {
    content: "";
    background-image: url("../assets/volume-high.svg");
    position: absolute;
    z-index: 2;
    left: 10px;
    top: 9px;
    width: 18px;
    height: 18px;
}

.my-button--volume-off::after {
    content: "";
    background-image: url("../assets/volume-off.svg");
    position: absolute;
    z-index: 2;
    left: 10px;
    top: 9px;
    width: 18px;
    height: 18px;
}
</style>