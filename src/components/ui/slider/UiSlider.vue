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
                   '_stretched': props.sliderParams.shouldStretchImages,
               },
           ]"
				>
				<slot></slot>
				</div>
			</div>
		</div>
<!--		<div class="swiper-pagination"></div>-->
<!--		<div class="swiper-scrollbar"></div>-->
		<UiSliderNavigation
				v-if="props.controls.hasNavigation"
				:position="props.controls.navigationPosition"
		/>
		<UiSliderPagination
				v-if="props.controls.hasPagination"
				:type="props.controls.paginationType"
		/>

		<div v-if="showControls"
		     class="count-buttons-line"
		>
			<UiSliderCount v-if="props.controls.showCount"
			               :active="countActive"
			               :all="props.slides.length"
			/>

		</div>
	</div>
</template>

<script setup lang="ts">
import Swiper from 'swiper/bundle';
import 'swiper/css/bundle';
import { ref, onMounted } from 'vue';
import UiSliderCount from '@/components/ui/slider/UiSliderCount.vue';
import UiSliderNavigation from '@/components/ui/slider/UiSliderNavigation.vue';
import UiSliderPagination from '@/components/ui/slider/UiSliderPagination.vue';
import { navigationButtonNextClass, navigationButtonPrevClass, paginationClass } from '@/components/ui/slider/constants';
import type { NavigationPosition } from '@/types';
// todo : add types from swiper

interface Slide {
	id: number,
}

type Control = 'navigation' | 'pagination';

interface Controls {
	hasPagination?: boolean,
	showCount?: boolean,
	hasNavigation?: boolean,
	pagination?: {},
	navigation?: {},
	navigationPosition?: NavigationPosition,
}

interface SliderParams {
	slidesPerView: number,
	loop: boolean,
	shouldStretchImages?: boolean,
	isLazy?: boolean,
}

interface Props {
	slides: Slide[],
	sliderParams?: SliderParams,
	controls?: Controls,
}

const props = withDefaults(defineProps<Props>(), {
	controls: {
		hasNavigation: true,
		hasPagination: true,
		showCount: true,
		pagination: {
			type: 'bullets',
		},
		navigationPosition: 'bottom-right',
	},
	sliderParams: {
		loop: false,
		slidesPerView: 1,
	},
});

const sliderElem = ref(null);
const showControls = ref(false);
const countActive = ref(1);

onMounted(() => {
	initSlider();
});

const initSlider = () => {
	if (!sliderElem.value || !props.slides.length) return;

	const slider = new Swiper(sliderElem.value, {
		preventInteractionOnTransition: true,
		touchEventsTarget: 'container',
		navigation: props.controls.hasNavigation ?
				{
					// * Класс можно укзаать любой - дефолтный от swiper или свой, без вложенности
					prevEl: `.${navigationButtonPrevClass}`,
					nextEl: `.${navigationButtonNextClass}`,
					...props.controls.navigation,
				} :
				{},
		pagination: props.controls.hasPagination ?
				{
					el: `.${paginationClass}`,
					...props.controls.pagination,
				} :
				{},
		// lazy: props.sliderParams.isLazy ?
		// 		{
		// 			checkInView: false,
		// 			loadOnTransitionStart: true,
		// 		} :
		// 		{},
		// ...props.sliderParams,
	});

	showControls.value = (slider.slides.length > 1);

	if (props.controls.hasPagination) {
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

const initControls = (controlType: Control) => {
	// todo: add UpperCaseFirstChar from utils
	const propsParam = `has${controlType}`;
	if (props.controls.hasNavigation) {
		setTimeout(() => {
			slider.update();
			slider.navigation.init()
			slider.navigation.update()
		}, 300);
	}
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


//.preloader {
//	position: absolute;
//	top: 0;
//	left: 0;
//	border: none;
//	width: 100%;
//	height: 100%;
//	margin: 0;
//	background-color: $grey_200;
//	border-radius: 0;
//	border-color: $grey_200;
//	overflow: hidden;
//	animation: none !important;
//
//	&:before {
//		content: "";
//		position: absolute;
//		top: -50%;
//		left: 0;
//		width: 100px;
//		height: 200%;
//		transform: skewX(-10deg);
//		background-image: linear-gradient(110deg, rgba(#fff, .7), rgba($grey_200, .7));
//		filter: blur(6px);
//		animation: translate-left 1.5s infinite;
//	}
//}
</style>
