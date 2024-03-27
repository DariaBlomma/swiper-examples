<template>
    <div
		    :class="[
						paginationClass,
						'ui-slider-pagination',
						`_${props.variant}`
				 ]"
    />
</template>

<script setup lang="ts">
import { paginationClass } from '@/components/ui/slider/constants';

interface Props {
	variant?: 'light' | 'dark',
}
const props = withDefaults(defineProps<Props>(), {
	variant: 'light',
});
</script>

<style lang="scss">
// просто scoped классы не работают без соответсвующей верстки
.ui-slider-pagination {
	&._light {
		.swiper-pagination-bullet {
			background: $grey_200;
		}

		.swiper-pagination-bullet-active {
			background: #fff;
		}
	}

	.swiper-pagination-progressbar {
		position: relative;
		flex-grow: 1;
		height: 2px;
		width: 100%;
		margin-right: 16px;
		border-radius: 60px;
		background-color: $grey_800;

		.swiper-pagination-progressbar-fill {
			background: $grey_100;
		}
	}
}
</style>

<style scoped lang="scss">
.ui-slider-pagination {
	//пример кастомизации с css variables, если нужна также прозрачность

	//--swiper-pagination-color: rgba(var(--test-base), 0.5);
}
</style>
