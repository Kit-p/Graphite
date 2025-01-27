<template>
	<div>
		<h3 class="example-pane-header">{{ name }}</h3>
		<div class="example-row">
			<div v-for="(example, index) in examples" :key="index">
				<BezierExample
					v-if="!example.disabled"
					:title="example.title"
					:points="example.points"
					:callback="callback"
					:sliderOptions="example.sliderOptions"
					:triggerOnMouseMove="triggerOnMouseMove"
				/>
			</div>
		</div>
	</div>
</template>

<script lang="ts">
import { defineComponent, PropType } from "vue";

import { BezierCallback, BezierCurveType, ExampleOptions, SliderOption } from "@/utils/types";

import BezierExample from "@/components/BezierExample.vue";

export default defineComponent({
	props: {
		name: String,
		callback: {
			type: Function as PropType<BezierCallback>,
			required: true,
		},
		exampleOptions: {
			type: Object as PropType<ExampleOptions>,
			default: () => ({}),
		},
		triggerOnMouseMove: {
			type: Boolean,
			default: false,
		},
	},
	data() {
		const exampleDefaults = {
			[BezierCurveType.Linear]: {
				points: [
					[30, 60],
					[140, 120],
				],
			},
			[BezierCurveType.Quadratic]: {
				points: [
					[30, 50],
					[140, 30],
					[160, 170],
				],
			},
			[BezierCurveType.Cubic]: {
				points: [
					[30, 30],
					[60, 140],
					[150, 30],
					[160, 160],
				],
			},
		};

		// Use quadratic slider options as a default if sliders are not provided for the other curve types.
		const defaultSliderOptions: SliderOption[] = this.exampleOptions[BezierCurveType.Quadratic]?.sliderOptions || [];

		return {
			examples: Object.values(BezierCurveType).map((curveType: BezierCurveType) => {
				const givenData = this.exampleOptions[curveType];
				const defaultData = exampleDefaults[curveType];
				return {
					title: curveType,
					disabled: givenData?.disabled || false,
					points: givenData?.customPoints || defaultData.points,
					sliderOptions: givenData?.sliderOptions || defaultSliderOptions,
				};
			}),
		};
	},
	components: {
		BezierExample,
	},
});
</script>

<style scoped>
.example-row {
	display: flex;
	flex-direction: row;
	justify-content: center;
}

.example-pane-header {
	margin-bottom: 0;
}
</style>
