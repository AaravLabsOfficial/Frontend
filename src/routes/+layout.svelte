<script lang="ts">
	import '../app.css';
	import backgroundImage from '@images/backgroundimagehelper.avif';
	let { children } = $props();
</script>
<!-- A background image overlay -->
<img src={backgroundImage} class="background-image-helper no-interact" alt="" />

{@render children()}