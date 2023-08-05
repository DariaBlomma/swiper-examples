<template>
    <div class="count">
        <span class="count__active">{{ activeCount }}</span>
        <span class="count__all">/ {{ allCount }}</span>
    </div>
</template>

<script setup lang="ts">
import { computed } from 'vue';

interface Props {
	all: number | string,
	active: number | string,
	/**
	 * Example
	 * true - 07
	 * false - 7
	 */
	hasZeroBasedFormat?: boolean,
}

const props = defineProps<Props>();

const activeCount = computed(() => {
	return props.hasZeroBasedFormat && Number(props.active) < 10 ?
			`0${props.active}` :
			props.active;
});

const allCount = computed(() => {
	return props.hasZeroBasedFormat && Number(props.all) < 10 ?
			`0${props.all}` :
			props.all;
});
</script>

<style scoped lang="scss">
.count {
	display: none;
    flex-shrink: 0;
    margin-right: 12px;
    padding: 5px 12px;
    border-radius: 50px;
    font-size: 14px;
    line-height: 22px;
    color: $grey_100;
    background-color: rgba($grey_300, .6);
    backdrop-filter: blur(10px);

    &__active {
        color: #fff;
    }
}

@media (max-width: 720px) {
	.count {
		margin-right: 0;
		padding: 2px 6px;
		font-size: 12px;
		line-height: 16px;
	}
}
</style>
