<template>
    <div class="swiper-pagination pagination" />
</template>

<script setup lang="ts">

</script>

<style lang="scss">
.swiper-pagination-progressbar {
    .swiper-pagination-progressbar-fill {
        background: $grey_100;
    }
}
</style>

<style scoped lang="scss">
.pagination {
    position: relative;
    flex-grow: 1;
    height: 2px;
    width: 100%;
    margin-right: 16px;
    border-radius: 60px;
    background-color: $grey_800;
}
</style>
