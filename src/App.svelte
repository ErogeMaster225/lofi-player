<script>
	// @ts-nocheck
	import { onMount } from "svelte";
	let youtubePlayer;
	let playerVolume = 100;
	let isPlaying = false;
	let isMuted = false;
	let playlistVisible = false;
	let current_index;
	let channel_index = 0;
	export const YOUTUBE_API = import.meta.env.VITE_YOUTUBE_API;
	import { channels } from "./assets/data.json";
	let playlist = [];
	let playlist1 = [
		{
			video_id: "jfKfPfyJRdk",
			video_title: "lofi hip hop radio - beats to relax/study to",
			video_thumbnail: "https://i.ytimg.com/vi/jfKfPfyJRdk/mqdefault_live.jpg",
			channel_name: "Lofi Girl",
		},
		{
			video_id: "rUxyKA_-grg",
			video_title: "lofi hip hop radio - beats to sleep/chill to",
			video_thumbnail: "https://i.ytimg.com/vi/rUxyKA_-grg/mqdefault_live.jpg",
			channel_name: "Lofi Girl",
		},
		{
			video_id: "5yx6BWlEVcY",
			video_title: "Chillhop Radio - jazzy & lofi hip hop beats 🐾",
			video_thumbnail: "https://i.ytimg.com/vi/5yx6BWlEVcY/mqdefault_live.jpg",
			channel_name: "Chillhop Music",
		},
		{
			video_id: "7NOSDKb0HlU",
			video_title: "lofi hip hop radio - beats to study/relax to 🐾",
			video_thumbnail: "https://i.ytimg.com/vi/7NOSDKb0HlU/mqdefault_live.jpg",
			channel_name: "Chillhop Music",
		},
		{
			video_id: "IjMESxJdWkg",
			video_title: "24/7 lofi hip hop radio - beats to study/chill/relax",
			video_thumbnail: "https://i.ytimg.com/vi/IjMESxJdWkg/mqdefault_live.jpg",
			channel_name: "nourish.",
		},
		{
			video_id: "BbQM9UbQjFI",
			video_title: "sad songs for sad people.",
			video_thumbnail: "https://i.ytimg.com/vi/BbQM9UbQjFI/mqdefault_live.jpg",
			channel_name: "nourish.",
		},
		{
			video_id: "MCkTebktHVc",
			video_title: "lofi hip hop radio - beats to relax/study to",
			video_thumbnail: "https://i.ytimg.com/vi/MCkTebktHVc/mqdefault_live.jpg",
			channel_name: "College Music",
		},
		{
			video_id: "XDh0JcxrbPM",
			video_title: "lofi hiphop radio | mellow/chill instrumental beats 🏔",
			video_thumbnail: "https://i.ytimg.com/vi/XDh0JcxrbPM/mqdefault_live.jpg",
			channel_name: "College Music",
		},
		{
			video_id: "gmv54pfxk0Q",
			video_title: "College Music · 24/7 Live Radio · Chilled Beats - Lofi | Electronic | Indie",
			video_thumbnail: "https://i.ytimg.com/vi/gmv54pfxk0Q/mqdefault_live.jpg",
			channel_name: "College Music",
		},
		{
			video_id: "bM0Iw7PPoU4",
			video_title: "vocal lofi hip hop radio - emotional/late night beats",
			video_thumbnail: "https://i.ytimg.com/vi/bM0Iw7PPoU4/mqdefault_live.jpg",
			channel_name: "College Music",
		},
		{
			video_id: "Otw7CXLqddM",
			video_title: "Summery Lofi 🍹 & Jazzy Hip Hop Live Radio 24/7 🏖",
			video_thumbnail: "https://i.ytimg.com/vi/Otw7CXLqddM/mqdefault_live.jpg",
			channel_name: "College Music",
		},
	];
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
					height: "360",
					width: "640",
					playerVars: {
						enablejsapi: 1,
						controls: 0,
						modestbranding: 1,
						disablekb: 1,
						iv_load_policy: 3,
						playsinline: 1,
					},
					events: {
						onReady: onPlayerReady,
						onStateChange: onPlayerStateChange,
					},
				});
			});
		}
		function onPlayerReady(event) {
			getLiveStream();
		}
		function onPlayerStateChange(event) {
			let videoStatuses = Object.entries(window.YT.PlayerState);
			console.log(videoStatuses.find((status) => status[1] === event.data)[0]);
		}
	};
	const setPlayback = () => {
		if (youtubePlayer.getPlayerState() == 1) {
			youtubePlayer.pauseVideo();
			isPlaying = false;
		} else {
			youtubePlayer.playVideo();
			isPlaying = true;
		}
	};
	const setVolume = () => {
		let slider = document.getElementById("volume-slider");
		slider.style.background = `linear-gradient(90deg, #fff ${playerVolume}%, #b7bcbf ${playerVolume + 0.1}%)`;
		youtubePlayer.setVolume(playerVolume);
	};
	const setMuted = () => {
		let slider = document.getElementById("volume-slider");
		if (youtubePlayer.isMuted() == 1) {
			youtubePlayer.unMute();
			isMuted = false;
			playerVolume = youtubePlayer.getVolume();
			slider.style.background = `linear-gradient(90deg, #fff ${playerVolume}%, #b7bcbf ${playerVolume + 0.1}%)`;
		} else {
			youtubePlayer.mute();
			isMuted = true;
			playerVolume = 0;
			slider.style.background = `linear-gradient(90deg, #fff ${playerVolume}%, #b7bcbf ${playerVolume + 0.1}%)`;
		}
	};
	const openVideo = (event) => {
		let youtube_id = event.target.getAttribute("data-id");
		let target = event.target;
		while (!youtube_id) {
			target = target.parentNode;
			youtube_id = target.getAttribute("data-id");
		}
		if (youtubePlayer.getPlayerState() == 1) {
			youtubePlayer.loadVideoById({ videoId: youtube_id });
		} else {
			youtubePlayer.cueVideoById({ videoId: youtube_id });
		}
		current_index = playllist.findIndex((object) => object.video_id == youtube_id);
		channel_index = channels.findIndex((object) => playlist[current_index].channel_name == object.name);
	};
	const openPrev = () => {
		if (current_index == 0) return;
		current_index--;
		let youtube_id = playlist[current_index].video_id;
		if (youtubePlayer.getPlayerState() == 1) {
			youtubePlayer.loadVideoById({ videoId: youtube_id });
		} else {
			youtubePlayer.cueVideoById({ videoId: youtube_id });
		}
		channel_index = channels.findIndex((object) => playlist[current_index].channel_name == object.name);
	};
	const openNext = () => {
		if (current_index == playlist.length - 1) return;
		current_index++;
		let youtube_id = playlist[current_index].video_id;
		if (youtubePlayer.getPlayerState() == 1) {
			youtubePlayer.loadVideoById({ videoId: youtube_id });
		} else {
			youtubePlayer.cueVideoById({ videoId: youtube_id });
		}
		channel_index = channels.findIndex((object) => playlist[current_index].channel_name == object.name);
	};

	const toggle_playlist = () => {
		playlistVisible = !playlistVisible;
		let iframe = document.getElementById("player");
		iframe.style.filter = playlistVisible == true ? "blur(3px)" : "";
	};
	const getLiveStreamRequest = (id) => {
		const response = fetch("https://youtube.googleapis.com/youtube/v3/search?part=snippet&channelId=" + id + "&channelType=any&eventType=live&type=video&key=" + YOUTUBE_API, {
			headers: {
				Accept: "application/json",
			},
		});
		return response;
	};
	const getLiveStream = () => {
		channels.forEach((channel) => {
			getLiveStreamRequest(channel.id)
				.then((response) => {
					if (response.ok) {
						return response.json();
					}
					throw new Error(response.statusText);
				})
				.then((data) => {
					data.items.forEach((item) => {
						playlist.push({
							video_id: item.id.videoId,
							video_title: item.snippet.title.replace(/&amp;/g, "&").replace(/&lt;/g, "<").replace(/&gt;/g, ">"),
							video_thumbnail: item.snippet.thumbnails.medium.url,
							channel_name: item.snippet.channelTitle,
						});
						playlist = playlist;
					});
                    youtubePlayer.cueVideoById({ videoId: playlist[0].video_id });
				})
				.catch((err) => {
					console.log(err);
				});
		});
		current_index = 0;
		channel_index = channels.findIndex((object) => playlist[current_index].channel_name == object.name);
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
		<div id="player" />
	</div>
	<div class:hide={isPlaying} class="youtube-pause fill" />
	<div class="radio-controls">
		<div class="center-space" />
		<button on:click={openPrev} class="control youtube-back">
			<div class="icons">
				<i class="fa-solid fa-backward-step" />
			</div>
		</button>
		<button on:click={setPlayback} class="control control-main">
			<div class="icons">
				<i class={isPlaying ? " fa-solid fa-pause" : " fa-solid fa-play"} />
			</div>
		</button>
		<button on:click={openNext} class="control youtube-forward">
			<div class="icons">
				<i class="fa-solid fa-forward-step" />
			</div>
		</button>
		<button on:click={setMuted} class="control youtube-volume">
			<div class="icons">
				<i class={isMuted ? "fa-solid fa-volume-xmark" : "fa-solid fa-volume"} />
			</div>
		</button>
		<div class="volume-slider">
			<input on:input={setVolume} bind:value={playerVolume} type="range" name="volume-slider" id="volume-slider" min="0" max="100" />
		</div>
	</div>
</div>
<div class="radio-overlay fill">
	<div class={playlistVisible ? "radio-list-container" : "hide-playlist radio-list-container"}>
		<div class="radio-list">
			{#each playlist as entry, i}
				<div class="video-entry" data-id={entry.video_id} on:click={openVideo}>
					<div class="entry-index">{i + 1}</div>
					<img src={entry.video_thumbnail} alt="" />
					<div class="entry-info">
						<div class="entry-title">{entry.video_title}</div>
						<div class="entry-channel">{entry.channel_name}</div>
					</div>
				</div>
			{/each}
		</div>
	</div>
	<div class="radio-details">
		<div class="channel-name">{channels[channel_index].name}</div>
		<!-- <div class="social-accounts">
			{#each Object.entries(channels[channel_index].social) as [account, link]}
                <a href="{link}">{account}</a>
			{/each}
		</div> -->
	</div>
	<i class={playlistVisible == true ? "fa-solid fa-eye" : "fa-solid fa-eye-slash"} on:click={toggle_playlist} />
</div>

<style>
	:root {
		font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, Oxygen, Ubuntu, Cantarell, "Open Sans", "Helvetica Neue", sans-serif;
	}
	#player {
		position: absolute;
		top: 50%;
		left: 50%;
		width: 100vw;
		height: 100vh;
		border: none;
		transform: translate(-50%, -50%);
		pointer-events: none;
	}
	@media (min-aspect-ratio: 16 / 9) {
		#player {
			height: 56.25vw;
		}
	}
	.hide {
		display: none !important;
	}
	.youtube-pause {
		background-color: rgba(0, 0, 0, 0.9);
		z-index: 2;
		position: absolute;
		top: 0;
		left: 0;
		overflow: hidden;
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
	.fa-solid {
		color: #fff;
	}
	.center-space {
		margin: 10px;
		width: 180px;
	}
	input[type="range"] {
		height: 5px;
		border-radius: 10px;
		-webkit-appearance: none;
		width: 100%;
		background: #fff;
	}
	input[type="range"]:focus {
		outline: none;
	}
	input[type="range"]::-webkit-slider-thumb {
		box-shadow: 1px 1px 1px #000000;
		border: 0px solid #000000;
		height: 20px;
		width: 20px;
		border-radius: 20px;
		background: #ffffff;
		cursor: pointer;
		-webkit-appearance: none;
	}
	input[type="range"]::-moz-range-thumb {
		box-shadow: 1px 1px 1px #000000;
		border: 0px solid #000000;
		height: 35px;
		width: 35px;
		border-radius: 50px;
		background: #ffffff;
		cursor: pointer;
	}
	.radio-list-container {
		display: flex;
		flex-direction: column;
		width: 30vw;
		height: 100vh;
		position: absolute;
		top: 0;
		left: 0;
		color: #fff;
		z-index: 10;
		scrollbar-width: 0;
		backdrop-filter: blur(10px);
		background: rgba(30, 30, 30, 0.4);
		transition: opacity 0.4s ease-in-out;
	}
	.hide-playlist {
		opacity: 0;
		pointer-events: none;
	}
	.radio-list {
		overflow: auto;
		flex: 1;
		padding-top: 15px;
	}

	::-webkit-scrollbar-thumb {
		background-color: rgba(0, 0, 0, 0.15) !important;
		border-radius: 10px;
	}
	::-webkit-scrollbar {
		background-color: transparent !important;
		width: 10px;
	}
	::-webkit-scrollbar-button {
		display: none;
	}
	.video-entry {
		padding: 10px 15px;
		margin: 0 10px 10px 0px;
		display: flex;
		position: relative;
		cursor: pointer;
	}
	.video-entry > img {
		width: 160px;
		height: 90px;
	}
	.video-entry .entry-info {
		position: relative;
		padding: 0px 15px;
		font-size: 11pt;
		flex: 1;
	}
	.video-entry .entry-title {
		-webkit-line-clamp: 2;
		-webkit-box-orient: vertical;
		overflow: hidden;
		display: -webkit-box;
	}
	.video-entry .entry-channel {
		position: absolute;
		bottom: 5px;
		left: 15px;
	}
	.video-entry > div {
		overflow: hidden;
		text-overflow: ellipsis;
	}
	.entry-index {
		display: flex;
		align-items: center;
		padding-right: 15px;
		text-shadow: 2px 4px 3px rgba(0, 0, 0, 0.8);
	}
	.fa-eye-slash,
	.fa-eye {
		position: absolute;
		top: 20px;
		right: 20px;
		z-index: 99;
	}
	.radio-details {
		position: absolute;
		right: 10%;
		bottom: 30%;
		z-index: 10;
	}
</style>
