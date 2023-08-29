<template>
	<HeadingX>Hello World</HeadingX>
	<HeadingScope tag="article">
		<p>
			Once upon a time, we said hello to the world.
		</p>

		<HeadingX>But then...</HeadingX>
		<HeadingScope level="4">
			<p>
				...we said hello to the world again.
			</p>

			<HeadingX level="1" html="And again...!" />
			<HeadingScope level="-2">
				<HeadingX :text="'And lastly...'" />
				<p>...and again.</p>
			</HeadingScope>
		</HeadingScope>
	</HeadingScope>
</template>
