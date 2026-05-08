<script lang="ts">
  import Hls from 'hls.js';

  let videoElement = $state<HTMLVideoElement | undefined>();
  const streamUrl = 'https://test-streams.mux.dev/x36xhzz/x36xhzz.m3u8';

  $effect(() => {
    if (!videoElement) return;

    if (Hls.isSupported()) {
      const hls = new Hls();
      hls.loadSource(streamUrl);
      hls.attachMedia(videoElement);
      hls.on(Hls.Events.MANIFEST_PARSED, () => {
        videoElement?.play();
      });

      return () => {
        hls.destroy();
      };
    } else if (videoElement.canPlayType('application/vnd.apple.mpegurl')) {
      // Soporte nativo (Safari, algunas Smart TVs modernas)
      videoElement.src = streamUrl;
      const onLoaded = () => videoElement?.play();
      videoElement.addEventListener('loadedmetadata', onLoaded);

      return () => {
        videoElement?.removeEventListener('loadedmetadata', onLoaded);
      };
    }
  });
</script>

<div class="tv-container">
  <header class="top-bar">
    <div class="placeholder-text">Svelte 5 TV Dashboard</div>
  </header>

  <main class="video-wrapper">
    <video
      bind:this={videoElement}
      autoplay
      muted
      playsinline
      loop
      class="video-player"
    ></video>
  </main>

  <footer class="bottom-bar">
    <div class="placeholder-ticker">Esperando datos de FastAPI...</div>
  </footer>
</div>

<style>
  :global(body, html) {
    margin: 0;
    padding: 0;
    background-color: #ffffff;
    overflow: hidden;
    height: 100vh;
    width: 100vw;
  }

  .tv-container {
    display: grid;
    grid-template-rows: 100px 1fr 100px;
    height: 100vh;
    width: 100vw;
    text-align: center;
  }

  .video-wrapper {
    display: flex;
    justify-content: center;
    align-items: center;
    background-color: #ffffff;
  }

  .video-player {
    width: 75%;
    aspect-ratio: 16 / 9;
    background: #000;
    box-shadow: 0 10px 50px rgba(0, 0, 0, 0.2);
    border-radius: 8px;
    border: 1px solid #eee;
  }

  .top-bar,
  .bottom-bar {
    display: flex;
    justify-content: center;
    align-items: center;
    font-family: 'Segoe UI', Roboto, Helvetica, Arial, sans-serif;
    color: #333;
    font-weight: bold;
    font-size: 1.5rem;
  }
</style>
