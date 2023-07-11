<template>
	<Component :is="tag" v-if="tag">
		<slot :heading-scope-level="level"></slot>
	</Component>
	<slot v-else :heading-scope-level="level"></slot>
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

provide('headingScopeLevel', level.value + 1);
</script>
