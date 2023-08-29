# HeadingX

Vue components for making headings and heading scopes for inserting the right tags and levels. The general concept is to be able to make components agnostic towards their nesting levels and use the right headings no matter where they are placed.

## Installation

``` bash
yarn add @limbo-works/heading-x
```

## Using the components

Make the component globally usable by extending the layer in `nuxt.config.js`.

``` js
export default defineNuxtConfig({
    extends: [
        '@limbo-works/heading-x',
        ...
    ],
    ...
});
```

Then you can use the HeadingX and `HeadingScope` components anywhere within that solution:

``` html
<!-- As written in Vue -->
<HeadingX>Hello World</HeadingX>
<HeadingScope tag="article">
	<p>Once upon a time, we said hello to the world.</p>

	<HeadingX>But then...</HeadingX>
	<HeadingScope level="4">
		<p>...we said hello to the world again.</p>

		<HeadingX level="1">And again...</HeadingX>
		<HeadingScope level="-1">
			<p>...and again.</p>
		</HeadingScope>
	</HeadingScope>
</HeadingScope>

<!-- As it may appear in the dom -->
<h1>Hello World</h1>
<article>
	<p>Once upon a time, we said hello to the world.</p>
	<h2>But then...</h2>
	<p>...we said hello to the world again.</p>
	<h1>And again...</h1>
	<p>...and again.</p>
</article>
```

**DON'T USE V-HTML OR V-TEXT!** Due to [a bug in Vue3](https://github.com/vuejs/core/issues/6435), these directives doesn't work properly in a SSR context when using dynamic components. If you need to use html, add a span inside and use it on that.

### Props overview

The same props are available for both `HeadingX` and `HeadingScope`.

| Prop | Description | Default value | Data type |
| ---- | ----------- | ------------- | --------- |
| tag | Per default, `HeadingX` will be rendered with the `h`-tag fitting its calculated heading level. If a tag is supplied it will be used instead. If it's a `h`-tag it will be used as-is, but any other tag will be supplied with role="heading" and a fitting aria-level.<br><br>HeadingScope will be tagless by default and will just render its content directly, but if a tag is supplied it will be used. | undefined | String |
| level | Setting the level for `HeadingX` will directly set the heading level, ie. a level of 1 will be `h1`, 2 `h2`, and so on. If not defined it will be calculated based on the upper `HeadingScope`s.<br><br>Setting a level for `HeadingScope` will set the heading scope level, ie. setting a level of 1, will make nested `HeadingX`s render as `h2`s (ie., the next headings after a `h1`), and so on. If not defined it will be calculated based on the upper `HeadingScope`s.<br><br>For both components you may prefix your number with "+" or "-" to set the value relatively to the calculated level. A value of "+1" on a `HeadingX` that would normally become a `h2` will, for example, become a `h3` instead. | undefined | Number\|String |

### Exposed slot props

Slot props only goes for `HeadingScope`.

| Prop | Description |
| ---- | ----------- |
| headingScopeLevel | Number representing the current scope. |

The same value is also provided by `HeadingScope` and can be used like: `const headingScopeLevel = inject('headingScopeLevel', 1);`