<script>
	import tilt from "./tilt";
</script>

<div use:tilt>
	<slot />
</div>

<style>
	div {
		text-align: center;
		padding: 2em;
		background-image: linear-gradient(to right, #001120, #00293a);
		max-width: 30em;
		margin: 3em auto;
		border-radius: 1.5em;
		cursor: grab;
		box-shadow: 0 0.5em 1em -0.25em #00000080;
		user-select: none;
		display: flex;
		flex-direction: column;
		justify-content: center;
	}
</style>
