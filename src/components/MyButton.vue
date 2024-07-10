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
        background-color: rgba(255, 110, 64, 0.3);
    }

    &:focus-visible {
        outline: none;
        border: 1px solid var(--colorPrimary);
        background-color: rgba(255, 110, 64, 0.4); 
    }

    &:active {
        background-color: rgba(255, 110, 64, 0.6);
    }
}


</style>