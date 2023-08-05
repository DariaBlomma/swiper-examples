<template>
	<div class="swiper" ref="sliderElem">
		<div class="swiper-wrapper">
			<div
					class="swiper-slide"
					v-for="slide in props.slides"
					:key="slide.id"
			>
				<div
						class="image"
						:class="[
               {
                   '_stretched': props.slider.shouldStretchImages,
               },
           ]"
				>
				<slot></slot>
				</div>
			</div>
		</div>
<!--		<div class="swiper-pagination"></div>-->

<!--		<div class="swiper-button-prev nav-button"></div>-->
<!--		<div class="swiper-button-next nav-button"></div>-->

<!--		<div class="swiper-scrollbar"></div>-->

		<div v-if="showCountButtonsControls"
		     class="count-buttons-line"
		     :class="`_${props.controls.countButtonsPosition}`"
		>
			<UiSliderCount v-if="props.controls.showCount"
			               :active="countActive"
			               :all="props.slides.length"
			/>
			<UiSliderButtons v-if="props.controls.hasNavigation" />
		</div>
		<UiSliderPagination v-if="props.controls.showPagination" />
	</div>
</template>

<script setup lang="ts">
import Swiper from 'swiper/bundle';
import 'swiper/css/bundle';
import { ref, onMounted } from 'vue';
import UiSliderCount from '@/components/ui/slider/UiSliderCount.vue';
import UiSliderButtons from '@/components/ui/slider/UiSliderButtons.vue';
import UiSliderPagination from '@/components/ui/slider/UiSliderPagination.vue';
import { navigationButtonNextClass, navigationButtonPrevClass } from '@/components/ui/slider/constants';

type CountPosition = 'bottom-right' | 'bottom-left' | 'top-left' | 'top-right';
//todo: have Navigation, Paginnation (boolean), pagination type fraction
interface Slide {
	id: number,
}

interface Slider {
	shouldStretchImages?: boolean,
	isLazy?: boolean,
}

interface Controls {
	showPagination?: boolean,
	showCount?: boolean,
	hasNavigation?: boolean,
	countButtonsPosition?: CountPosition,
	paginationType?: null,
}

interface Params {
	slidesPerView: number,
	loop: boolean,
}

interface Props {
	slides: Slide[],
	slider?: Slider,
	controls?: Controls,
	sliderParams?: Params,
}
const props = withDefaults(defineProps<Props>(), {
	slider: {},
	controls: {
		countButtonsPosition: 'bottom-right',
		hasNavigation: true,
		showCount: true,
	},
	sliderParams: {
		loop: false,
		slidesPerView: 1,
	},
});

const sliderElem = ref(null);
const showCountButtonsControls = ref(false);
const countActive = ref(1);

onMounted(() => {
	initSlider();
});

const initSlider = () => {
	if (!sliderElem.value || !props.slides.length) return;

	const slider = new Swiper(sliderElem.value, {
		preventInteractionOnTransition: true,
		touchEventsTarget: 'container',
		navigation: props.controls.hasNavigation ? {
			prevEl: `.${navigationButtonPrevClass}`,
			nextEl: `.${navigationButtonNextClass}`,
		} :
		{},
		// pagination: props.controls.paginationType ? {
		// 			type: props.controls.paginationType,
		// 			el: '.swiper-pagination',
		// 		} :
		// 		{},
		// lazy: props.slider.isLazy ?
		// 		{
		// 			checkInView: false,
		// 			loadOnTransitionStart: true,
		// 		} :
		// 		{},
		// ...props.sliderParams,
	});

	showCountButtonsControls.value = (slider.slides.length > 1);

	if (props.controls.showPagination) {
		setTimeout(() => {
			slider.update();
			slider.pagination.render()
			slider.pagination.update()
		}, 300);
	}

	if (props.controls.hasNavigation) {
		setTimeout(() => {
			slider.update();
			slider.navigation.init()
			slider.navigation.update()
		}, 300);
	}

	slider.on('slideChange', () => {
		countActive.value = slider.realIndex + 1;
	});
};
</script>

<style scoped lang="scss">
.swiper {
	width: 500px;
	min-height: 300px;
	margin: 0 auto;
	border-radius: inherit;
	overflow: hidden;
}

.swiper-slide {
	width: 100%;
}

.image {
	width: 100%;
	min-height: 300px;
	height: 100%;
	background-image: url("../../../assets/images/guitar.jpg");
	background-position: center;
	background-size: contain;
	background-repeat: no-repeat;
	background-color: $blue_500;

	&._stretched {
		background-size: cover;
	}
}

.preloader {
	position: absolute;
	top: 0;
	left: 0;
	border: none;
	width: 100%;
	height: 100%;
	margin: 0;
	background-color: $grey_200;
	border-radius: 0;
	border-color: $grey_200;
	overflow: hidden;
	animation: none !important;

	&:before {
		content: "";
		position: absolute;
		top: -50%;
		left: 0;
		width: 100px;
		height: 200%;
		transform: skewX(-10deg);
		background-image: linear-gradient(110deg, rgba(#fff, .7), rgba($grey_200, .7));
		filter: blur(6px);
		animation: translate-left 1.5s infinite;
	}
}

.count-buttons-line {
	position: absolute;
	z-index: 1;
	display: flex;
	align-items: center;
	width: fit-content;
	height: fit-content;
	opacity: 1;
	transition: opacity .3s ease-in-out;

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
</style>
