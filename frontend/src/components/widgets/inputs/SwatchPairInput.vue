<template>
	<LayoutCol class="swatch-pair">
		<LayoutRow class="secondary swatch">
			<button @click="() => clickSecondarySwatch()" :style="`--swatch-color: ${secondary.toRgbaCSS()}`" data-hover-menu-spawner></button>
			<ColorPicker v-model:open="secondaryOpen" :color="secondary" @update:color="(color: Color) => secondaryColorChanged(color)" :direction="'Right'" />
		</LayoutRow>
		<LayoutRow class="primary swatch">
			<button @click="() => clickPrimarySwatch()" :style="`--swatch-color: ${primary.toRgbaCSS()}`" data-hover-menu-spawner></button>
			<ColorPicker v-model:open="primaryOpen" :color="primary" @update:color="(color: Color) => primaryColorChanged(color)" :direction="'Right'" />
		</LayoutRow>
	</LayoutCol>
</template>

<style lang="scss">
.swatch-pair {
	// Reversed order of elements paired with `column-reverse` allows primary to overlap secondary without relying on `z-index`
	flex-direction: column-reverse;
	flex: 0 0 auto;

	.swatch {
		width: 28px;
		height: 28px;
		margin: 0 2px;
		position: relative;

		button {
			--swatch-color: #ffffff;
			width: 100%;
			height: 100%;
			border-radius: 50%;
			border: 2px var(--color-7-middlegray) solid;
			box-shadow: 0 0 0 2px var(--color-3-darkgray);
			margin: 0;
			padding: 0;
			box-sizing: border-box;
			outline: none;
			background: linear-gradient(45deg, #cccccc 25%, transparent 25%, transparent 75%, #cccccc 75%), linear-gradient(45deg, #cccccc 25%, transparent 25%, transparent 75%, #cccccc 75%),
				linear-gradient(#ffffff, #ffffff);
			background-size: 16px 16px;
			background-position: 0 0, 8px 8px;
			overflow: hidden;

			&::before {
				content: "";
				display: block;
				width: 100%;
				height: 100%;
				background: var(--swatch-color);
			}
		}

		.floating-menu {
			top: 50%;
			right: -2px;
		}

		&.primary {
			margin-bottom: -8px;
		}
	}
}
</style>

<script lang="ts">
import { defineComponent, type PropType } from "vue";

import { type Color } from "@/wasm-communication/messages";

import ColorPicker from "@/components/floating-menus/ColorPicker.vue";
import LayoutCol from "@/components/layout/LayoutCol.vue";
import LayoutRow from "@/components/layout/LayoutRow.vue";

export default defineComponent({
	inject: ["editor"],
	props: {
		primary: { type: Object as PropType<Color>, required: true },
		secondary: { type: Object as PropType<Color>, required: true },
	},
	data() {
		return {
			primaryOpen: false,
			secondaryOpen: false,
		};
	},
	methods: {
		clickPrimarySwatch() {
			this.primaryOpen = true;
			this.secondaryOpen = false;
		},
		clickSecondarySwatch() {
			this.primaryOpen = false;
			this.secondaryOpen = true;
		},
		primaryColorChanged(color: Color) {
			this.editor.instance.updatePrimaryColor(color.red, color.green, color.blue, color.alpha);
		},
		secondaryColorChanged(color: Color) {
			this.editor.instance.updateSecondaryColor(color.red, color.green, color.blue, color.alpha);
		},
	},
	components: {
		ColorPicker,
		LayoutCol,
		LayoutRow,
	},
});
</script>
