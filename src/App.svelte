<script>
	// @ts-nocheck
	let youtubePlayer;
	const loadVideo = () => {
		(function loadYoutubeIFrameApiScript() {
			const tag = document.createElement("script");
			tag.src = "https://www.youtube.com/iframe_api";
			const firstScriptTag = document.getElementsByTagName("script")[0];
			firstScriptTag.parentNode.insertBefore(tag, firstScriptTag);
			tag.onload = setupPlayer;
		})();
		function setupPlayer() {
			YT.ready(function () {
				youtubePlayer = new YT.Player("player", {
					events: {
						onReady: onPlayerReady,
						onStateChange: onPlayerStateChange,
					},
				});
			});
		}
		function onPlayerReady(event) {
			event.target.playVideo();
		}
		function onPlayerStateChange(event) {
			let videoStatuses = Object.entries(window.YT.PlayerState);
			console.log(videoStatuses.find((status) => status[1] === event.data)[0]);
		}
	};
	const handleClick = () => {
		console.log("stopped");
		youtubePlayer.pauseVideo();
	};
	if (document.readyState !== "loading") {
		console.info(`document.readyState ==>`, document.readyState);
		loadVideo();
	} else {
		document.addEventListener("DOMContentLoaded", function () {
			console.info(`DOMContentLoaded ==>`, document.readyState);
			loadVideo();
		});
	}
</script>

<div class="radio-player youtube fill">
	<div class="youtube-video">
		<iframe
			id="player"
			width="640"
			height="360"
			src="https://www.youtube.com/embed/7NOSDKb0HlU?controls=0&modestbranding=1&disablekb=1&enablejsapi=1&iv_load_policy=3&playsinline=1"
			title="YouTube video player"
			frameborder="0"
			allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture"
			allowfullscreen
		/>
	</div>
	<div class="radio-controls">
		<div class="center-space" />
		<button class="control youtube-back"><div class="icons"><i class="fa-solid fa-backward-step" /></div></button>
		<button on:click={handleClick} class="control control-main"><div class="icons"><i class="fa-solid fa-play" /></div></button>
		<button class="control youtube-forward"><div class="icons"><i class="fa-solid fa-forward-step" /></div></button>
		<button class="control youtube-volume"><div class="icons"><i class="fa-solid fa-volume" /></div></button>
		<div class="volume-slider">
			<input type="range" name="volume-slider" id="volume-slider" min="0" max="100" />
		</div>
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
		width: 100%;
		margin-bottom: 20px;
	}
	.control {
		background-color: transparent;
		border-radius: 50%;
		border: 2px solid #fff;
		margin: 10px;
		padding: 20px;
		cursor: pointer;
	}
	.control-main {
		padding: 35px;
	}
	.icons {
		display: block;
		width: 15px;
		height: 15px;
		text-align: center;
	}
	.center-space {
		margin: 10px;
		width: 180px;
	}
</style>
