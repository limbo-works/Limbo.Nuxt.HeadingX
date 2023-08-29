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
		html: {
			type: String,
			default: undefined,
		},
		text: {
			type: String,
			default: undefined,
		},
	},
	data() {
		return {
			internalHtml: '',
			internalText: '',
		};
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
	// All this may seem a bit overkill, but it's necessary to make sure v-html and v-text work
	watch: {
		html() {
			this.updateInternalValues();
		},
		text() {
			this.updateInternalValues();
		},
	},
	mounted() {
		this.updateInternalValues();
	},
	methods: {
		updateInternalValues() {
			this.internalHtml = this.html;
			this.internalText = this.text;
		},
	},
	render() {
		if (this.html || this.text) {
			return h(this.computedTag, {
				role: this.isRealHeading ? null : 'heading',
				'aria-level': this.isRealHeading ? null : this.computedLevel,
				...this.$attrs,
				innerHTML: this.internalHtml || this.html,
				textContent: this.internalText || this.text,
			});
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
