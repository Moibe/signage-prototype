<script lang="ts">
  const embedUrl = 'http://192.168.100.208:5174/';
</script>

<div class="tv-container">
  <header class="top-bar">
    <div class="placeholder-text">Svelte 5 TV Dashboard</div>
  </header>

  <main class="video-wrapper">
    <iframe
      src={embedUrl}
      title="Embedded app"
      class="embed-frame"
    ></iframe>
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
    position: relative;
    display: flex;
    justify-content: center;
    align-items: center;
    background-color: #ffffff;
  }

  .embed-frame {
    width: 75%;
    aspect-ratio: 16 / 9;
    background: #fff;
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
