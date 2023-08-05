<template>
    <div
		    class="navigation"
		    :class="`_${props.position}`"
    >
      <div :class="navigationButtonNextClass">
        <UiSliderNavArrow variant="right" />
      </div>
	    <div :class="navigationButtonPrevClass">
		    <UiSliderNavArrow />
	    </div>
    </div>
</template>

<script setup lang="ts">
import UiSliderNavArrow from '../icons/UiArrowIcon.vue';
import { navigationButtonNextClass, navigationButtonPrevClass } from '@/components/ui/slider/constants';
import type { NavigationPosition } from '@/types';

interface Props {
	position?: NavigationPosition,
}
const props = defineProps<Props>();
</script>

<style scoped lang="scss">
.navigation {
	position: absolute;
	z-index: 1;
	display: flex;
	flex-direction: row-reverse;
	flex-shrink: 0;
	gap: 8px;

	&._bottom-right {
		right: 24px;
		bottom: 24px;
	}

	&._bottom-left {
		left: 24px;
		bottom: 24px;
	}

	&._top-right {
		right: 24px;
		top: 24px;
	}

	&._top-left {
		left: 24px;
		top: 24px;
	}
}

.nav-button-prev, .nav-button-next {
	display: inline-flex;
	justify-content: center;
	align-items: center;
	width: 32px;
	height: 32px;
	background-color: #fff;
	border-radius: 40px;
	font-size: 14px;
	font-weight: 300;
	line-height: 1.21;
	text-align: center;
	text-decoration: none;
	border: none;
	color: #000;
	transition: all 0.4s ease;
	outline: none;
	cursor: pointer;
	user-select: none;

	&:hover {
		background-color: darken($grey_200, 20%);
	}

	&:active {
		background-color: $grey_300;
	}
}

.swiper-button-disabled {
		opacity: 0.5;
    cursor: auto;
    pointer-events: none;
}
</style>
