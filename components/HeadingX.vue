<!--
	Last modified: 2023/08/28 13:49:30
-->
<template>
	<Component
		:is="tag"
		:role="isRealHeading ? null : 'heading'"
		:aria-level="isRealHeading ? null : level"
		v-bind="{
			innerHTML,
			textContent,
		}"
	>
		<slot></slot>
	</Component>
</template>

<script setup>
const headingScopeLevel = inject('headingScopeLevel', 1);

const props = defineProps({
	tag: {
		type: String,
		default: undefined,
	},
	level: {
		type: [Number, String],
		default: undefined,
	},
	// For v-html and v-text to work...
	textContent: String,
	innerHTML: String,
});

const level = computed(() => {
	if (props.level || typeof props.level === 'number') {
		// Add one to headingScopeLevel
		if (typeof props.level === 'string' && props.level.startsWith('+')) {
			return Number(props.level.substr(1)) + headingScopeLevel;
		}
		// Subtract one from headingScopeLevel
		if (typeof props.level === 'string' && props.level.startsWith('-')) {
			return headingScopeLevel - Number(props.level.substr(1));
		}
		// Plain level
		return Number(props.level);
	}
	return headingScopeLevel;
});

const tag = computed(() => {
	if (props.tag) {
		return props.tag;
	}
	if (level.value >= 1 && level.value <= 6) {
		return `h${level.value}`;
	}
	return 'div';
});

const isRealHeading = computed(() => {
	return ['h1', 'h2', 'h3', 'h4', 'h5', 'h6'].includes(tag.value);
});
</script>
