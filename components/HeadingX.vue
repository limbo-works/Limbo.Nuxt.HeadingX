<script>
export default {
	name: 'HeadingX',
	inject: {
		headingScopeLevel: {
			default: 1,
		},
	},
	inheritAttrs: false,
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
	computed: {
		computedLevel() {
			if (this.level || typeof this.level === 'number') {
				// Add one to headingScopeLevel
				if (
					typeof this.level === 'string' &&
					this.level.startsWith('+')
				) {
					return (
						Number(this.level.substr(1)) + this.headingScopeLevel
					);
				}
				// Subtract one from headingScopeLevel
				if (
					typeof this.level === 'string' &&
					this.level.startsWith('-')
				) {
					return (
						this.headingScopeLevel - Number(this.level.substr(1))
					);
				}
				// Plain level
				return Number(this.level);
			}
			return this.headingScopeLevel;
		},
		computedTag() {
			if (this.tag) {
				return this.tag;
			}
			if (this.computedLevel >= 1 && this.computedLevel <= 6) {
				return `h${this.computedLevel}`;
			}
			return 'div';
		},
		isRealHeading() {
			return ['h1', 'h2', 'h3', 'h4', 'h5', 'h6'].includes(
				this.computedTag
			);
		},
	},
	render() {
		if (this.$attrs.innerHTML || this.$attrs.innerText) {
			return h(
				this.computedTag,
				{
					role: this.isRealHeading ? null : 'heading',
					'aria-level': this.isRealHeading ? null : this.computedLevel,
					...this.$attrs,
					innerHTML: null,
					innerText: null,
				},
				this.$attrs.innerHTML ? h(
					'span',
					{
						innerHTML: this.$attrs.innerHTML,
					},
				) : this.$attrs.innerText,
			);
		}
		return h(
			this.computedTag,
			{
				role: this.isRealHeading ? null : 'heading',
				'aria-level': this.isRealHeading ? null : this.computedLevel,
				...this.$attrs,
			},
			this.$slots.default?.()
		);
	},
};
</script>
