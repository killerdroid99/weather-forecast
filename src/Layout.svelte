<div />
<slot />

<style>
	div {
		position: absolute;
		height: 100%;
		width: 100%;
		top: 0;
		background-image: url("./bg.svg");
		z-index: -1;
		background-size: cover;
		background-position: center;
		background-repeat: no-repeat;
		margin: 0;
	}
</style>
