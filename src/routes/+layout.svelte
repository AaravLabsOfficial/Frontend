<script lang="ts">
	import '../app.css';
	import backgroundImage from '@images/backgroundimagehelper.avif';
	let { children } = $props();
</script>

<img src={backgroundImage} class="background-image-helper no-interact" alt="" />

{@render children()}

<style>
	.background-image-helper {
		position: fixed;
		top: 0;
		left: 0;
		width: 100%;
		z-index: -5;
	}
</style>