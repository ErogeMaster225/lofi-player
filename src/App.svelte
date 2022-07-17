<script>
</script>

<div class="radio-player youtube fill">
	<div class="youtube-video">
		<iframe
			width="640"
			height="360"
			src="https://www.youtube.com/embed/7NOSDKb0HlU?autoplay=1&controls=0&modestbranding=1&disablekb=1&enablejsapi=1&iv_load_policy=3&playsinline=1"
			title="YouTube video player"
			frameborder="0"
			allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture"
			allowfullscreen
		/>
	</div>
	<div class="radio-controls">
		<div class="control" />
	</div>
</div>
<div class="radio-overlay" />

<style>
	:root {
		font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, Oxygen, Ubuntu, Cantarell, "Open Sans", "Helvetica Neue", sans-serif;
	}
	iframe {
		position: absolute;
		top: 50%;
		left: 50%;
		width: 100vw;
		height: 100vh;
		border: none;
		transform: translate(-50%, -50%);
		/* pointer-events: none; */
	}
	@media (min-aspect-ratio: 16 / 9) {
		iframe {
			height: 56.25vw;
		}
	}
	.fill {
		width: 100%;
		min-width: 100%;
		max-width: 100%;
		height: 100%;
		min-height: 100%;
		max-height: 100%;
	}
	.youtube::after {
		background: rgb(0, 0, 0);
		background: linear-gradient(0deg, rgba(0, 0, 0, 1) 0%, rgba(0, 0, 0, 0) 20%, rgba(0, 0, 0, 0) 80%, rgba(0, 0, 0, 1) 100%);
		position: absolute;
		top: 0;
		left: 0;
		width: 100%;
		height: 100%;
		content: "";
		overflow: hidden;
		pointer-events: none;
	}
	.radio-controls {
		display: flex;
		justify-content: center;
		align-items: center;
		position: absolute;
		left: 0;
		bottom: 0;
		z-index: 5;
		overflow: hidden;
	}
</style>
