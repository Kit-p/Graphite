<template>
	<span class="text-label" :class="{ bold, italic, multiline, 'table-align': tableAlign }" :style="minWidth > 0 ? `min-width: ${minWidth}px` : ''" :title="tooltip">
		<slot></slot>
	</span>
</template>

<style lang="scss">
.text-label {
	line-height: 18px;
	white-space: nowrap;

	&.bold {
		font-weight: 700;
	}

	&.italic {
		font-style: italic;
	}

	&.multiline {
		white-space: pre-wrap;
		margin: 4px 0;
	}

	&.table-align {
		flex: 0 0 30%;
		text-align: right;
	}
}
</style>

<script lang="ts">
import { defineComponent, type PropType } from "vue";

export default defineComponent({
	props: {
		bold: { type: Boolean as PropType<boolean>, default: false },
		italic: { type: Boolean as PropType<boolean>, default: false },
		tableAlign: { type: Boolean as PropType<boolean>, default: false },
		minWidth: { type: Number as PropType<number>, default: 0 },
		multiline: { type: Boolean as PropType<boolean>, default: false },
		tooltip: { type: String as PropType<string | undefined>, required: false },
	},
});
</script>
