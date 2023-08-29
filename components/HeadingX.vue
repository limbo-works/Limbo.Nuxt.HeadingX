<script>
export default {
	name: 'HeadingX',
	props: {
		tag: {
			type: String,
			default: undefined,
		},
		level: {
			type: [Number, String],
			default: undefined,
		},
		html: {
			type: String,
			default: undefined,
		},
		text: {
			type: String,
			default: undefined,
		},
	},
	render() {
		const headingScopeLevel = inject('headingScopeLevel', 1);

		const level = computed(() => {
			if (this.level || typeof this.level === 'number') {
				// Add one to headingScopeLevel
				if (typeof this.level === 'string' && this.level.startsWith('+')) {
					return Number(this.level.substr(1)) + headingScopeLevel;
				}
				// Subtract one from headingScopeLevel
				if (typeof this.level === 'string' && this.level.startsWith('-')) {
					return headingScopeLevel - Number(this.level.substr(1));
				}
				// Plain level
				return Number(this.level);
			}
			return headingScopeLevel;
		});
		const tag = computed(() => {
			if (this.tag) {
				return this.tag;
			}
			if (level.value >= 1 && level.value <= 6) {
				return `h${level.value}`;
			}
			return 'div';
		});

		const isRealHeading = ['h1', 'h2', 'h3', 'h4', 'h5', 'h6'].includes(tag.value);

		if (this.html || this.text) {
			return h(tag.value, {
				role: isRealHeading ? null : 'heading',
				'aria-level': isRealHeading ? null : level.value,
				...this.$attrs,
				innerHTML: this.html,
				textContent: this.text,
			});
		}
		return h(tag.value, {
			role: isRealHeading ? null : 'heading',
			'aria-level': isRealHeading ? null : level.value,
			...this.$attrs,
		}, this.$slots.default?.());
	},
};
</script>
