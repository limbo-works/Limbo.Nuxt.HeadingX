<script>
export default {
	name: 'HeadingX',
	inject: {
		headingScopeLevel: {
			default: 1,
		},
	},
	props: {
		tag: {
			type: String,
			default: undefined,
		},
		level: {
			type: [Number, String],
			default: undefined,
		},
	},
	setup(props, { attrs, slots }) {
		const headingScopeLevel = inject('headingScopeLevel', 1);

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

		const isRealHeading = ['h1', 'h2', 'h3', 'h4', 'h5', 'h6'].includes(tag.value);

		return () => h(tag.value, {
			role: isRealHeading ? null : 'heading',
			'aria-level': isRealHeading ? null : level.value,
			...attrs,
		}, slots.default?.());
	},
};
</script>
