<template>
    <svg
		    class="svg"
		    :class="`_${props.variant}`"
		    width="16"
		    height="16"
		    viewBox="0 0 16 16"
		    fill="none"
		    xmlns="http://www.w3.org/2000/svg"
    >
        <path
		        class="path"
		        d="M7.21834 8.00011L10.5183 11.3001L9.57567 12.2428L5.33301 8.00011L9.57567 3.75745L10.5183 4.70011L7.21834 8.00011Z"
        />
    </svg>
</template>

<script setup lang="ts">
interface Props {
	variant?: 'left' | 'right',
}
const props = withDefaults(defineProps<Props>(), {
	variant: 'left',
});
</script>

<style scoped lang="scss">
.svg {
	&._right {
		transform: rotate(180deg);
	}
}

.path {
	fill: $grey_300;
}
</style>
