<template>
    <header class="header">
        <a href="#" class="brand">
            KANBOARD
        </a>
    </header>
</template>

<script>
export default {
    name: "Header",
};
</script>

<style scoped>
.header {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    padding: 15px 15px;
    height: 4.5rem;
    display: flex;
    align-items: center;
    z-index: 0;
    background: var(--color-purple);
}

.brand {
    position: relative;
    font-size: 3rem;
    padding: 1rem;
    text-decoration: none;
    color: var(--color-white);
    font-weight: 400;
}
</style>