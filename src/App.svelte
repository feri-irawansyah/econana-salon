<script>
  import svelteLogo from './assets/svelte.svg'
  import viteLogo from '/vite.svg'
  import Counter from './lib/Counter.svelte'

  import { onMount } from "svelte";
  let progress = 0;
  let total = 0;
  let message = "";
  let done = false;

  onMount(() => {
      const ws = new WebSocket("ws://localhost:8000/api/v1/generic/ws/");

      ws.onmessage = (event) => {
          const msg = JSON.parse(event.data);

          if (msg.event === "import_progress") {
              progress = msg.data.current;
              total = msg.data.total;
              message = `Importing ${msg.data.row} (${progress} of ${total})`;
          }

          if (msg.event === "import_done") {
              message = "Import selesai!";
              done = true;
          }
      };
  });

  async function importData() {
    total = 0;
    progress = 0;
      await fetch("http://localhost:8000/api/v1/data/import", {
      method: "POST",
      credentials: "include",
      headers: {
          "Content-Type": "application/json"
      },
      body: JSON.stringify([
          { id: 1, name: "Data 1" },
          { id: 2, name: "Data 2" },
          { id: 3, name: "Data 3" },
          { id: 4, name: "Data 4" },
          { id: 5, name: "Data 5" },
          { id: 6, name: "Data 6" },
          { id: 7, name: "Data 7" },
          { id: 8, name: "Data 8" },
          { id: 9, name: "Data 9" },
          { id: 10, name: "Data 10" },
          { id: 11, name: "Data 11" },
          { id: 12, name: "Data 12" },
          { id: 13, name: "Data 13" },
          { id: 14, name: "Data 14" },
          { id: 15, name: "Data 15" },
          { id: 16, name: "Data 16" },
          { id: 17, name: "Data 17" },
          { id: 18, name: "Data 18" },
          { id: 19, name: "Data 19" },
          { id: 20, name: "Data 20" }
      ])
    })
    .then(res => res.json())
    .then(data => {
        console.log(data)
    });

  }

</script>

<main>
  <div>
    <a href="https://vite.dev" target="_blank" rel="noreferrer">
      <img src={viteLogo} class="logo" alt="Vite Logo" />
    </a>
    <a href="https://svelte.dev" target="_blank" rel="noreferrer">
      <img src={svelteLogo} class="logo svelte" alt="Svelte Logo" />
    </a>
  </div>
  <h1>Ekonana Salon</h1>
  <p>Onprogress....</p>
  <div class="card">
    <Counter />
  </div>

  <p>
    <button on:click={importData}>Import Data</button>
  </p>
<div class="my-3">
    <div class="progress">
        <div
            class="progress-bar progress-bar-striped progress-bar-animated bg-success"
            role="progressbar"
            style="width: {total > 0 ? (progress / total) * 100 : 0}%"
            aria-valuenow={progress}
            aria-valuemin="0"
            aria-valuemax={total}>
            {progress} / {total}
        </div>
    </div>

    <p class="mt-2">{message}</p>
</div>
  <p>
    Check out <a href="https://github.com/sveltejs/kit#readme" target="_blank" rel="noreferrer">SvelteKit</a>, the official Svelte app framework powered by Vite!
  </p>

  <p class="read-the-docs">
    Click on the Vite and Svelte logos to learn more
  </p>
</main>

<style>
  .logo {
    height: 6em;
    padding: 1.5em;
    will-change: filter;
    transition: filter 300ms;
  }
  .logo:hover {
    filter: drop-shadow(0 0 2em #646cffaa);
  }
  .logo.svelte:hover {
    filter: drop-shadow(0 0 2em #ff3e00aa);
  }
  .read-the-docs {
    color: #888;
  }
</style>
