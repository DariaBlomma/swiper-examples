<template>
    <div class="navigation">
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

</script>

<style scoped lang="scss">
//todo - how to customize? maybe a set of available size, small, middle, large?

.navigation {
	display: flex;
	flex-direction: row-reverse;
	flex-shrink: 0;
	gap: 8px;
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
