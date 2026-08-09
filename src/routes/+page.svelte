<script lang="ts">
  const videos = ['/videos_web/clip1.mp4', '/videos_web/clip2.mp4'];
  let current = $state(0);
  let status = $state('arrancando...');
  let videoEl: HTMLVideoElement;

  function playCurrent() {
    status = `cargando ${videos[current]}`;
    videoEl.src = videos[current];
    videoEl.load();
    videoEl
      .play()
      .then(() => (status = `reproduciendo ${videos[current]}`))
      .catch((err) => (status = `play() rechazado en ${videos[current]}: ${err.name}: ${err.message}`));
  }

  function next() {
    current = (current + 1) % videos.length;
    playCurrent();
  }

  function onError() {
    const err = videoEl.error;
    status = `error en ${videos[current]}: code ${err?.code} ${err?.message ?? ''}`;
    next();
  }
</script>

<div class="tv-container">
  <header class="top-bar">
    <div class="placeholder-text">Svelte 5 TV Dashboard</div>
  </header>

  <main class="video-wrapper">
    <video
      bind:this={videoEl}
      src={videos[0]}
      class="embed-frame"
      autoplay
      muted
      playsinline
      controls
      onended={next}
      onerror={onError}
      onplaying={() => (status = `reproduciendo ${videos[current]}`)}
    ></video>
  </main>

  <footer class="bottom-bar">
    <div class="placeholder-ticker">{status}</div>
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
    position: relative;
    display: flex;
    justify-content: center;
    align-items: center;
    background-color: #ffffff;
  }

  .embed-frame {
    width: 100%;
    height: 100%;
    object-fit: cover;
    background: #000;
  }

  :global(.embed-frame::-webkit-media-controls-panel) {
    opacity: 1 !important;
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
