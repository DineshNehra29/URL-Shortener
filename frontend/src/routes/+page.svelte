<script lang="ts">
    import axios from "axios";
  
    let url: string = "";
    let shortUrl: string = "";
    let loading: boolean = false;
    let error: string = "";
    let previousLinks: { original: string; short: string }[] = [];
  
    async function shortenURL() {
      if (!url.trim()) {
        error = "Please enter a valid URL!";
        return;
      }
  
      loading = true;
      error = "";
      shortUrl = "";
  
      try {
        const response = await axios.post("http://localhost:3001/shorten", { url });
        shortUrl = response.data.short_url;
        
        
        previousLinks = [...previousLinks, { original: url, short: shortUrl }];
        url = ""; 
      } catch (err) {
        error = "Failed to shorten URL. Please try again!";
      } finally {
        loading = false;
      }
    }
  </script>
  
  
  <style>
    .loading {
      color: blue;
      font-weight: bold;
    }
    .error {
      color: red;
    }
    .links {
      margin-top: 20px;
    }
  </style>
  
  <h1>URL Shortener</h1>
  
  <input type="text" bind:value={url} placeholder="Enter URL" />
  <button on:click={shortenURL} disabled={loading}>Shorten</button>
  
  {#if loading}
    <p class="loading">Shortening URL...</p>
  {/if}
  
  {#if error}
    <p class="error">{error}</p>
  {/if}
  
  {#if shortUrl}
    <p>Shortened URL: <a href="{shortUrl}" target="_blank">{shortUrl}</a></p>
  {/if}
  
  <h2>Previously Shortened Links</h2>
  <ul class="links">
    {#each previousLinks as link}
      <li>
        <strong>Original:</strong> <a href="{link.original}" target="_blank">{link.original}</a>
        <br />
        <strong>Shortened:</strong> <a href="{link.short}" target="_blank">{link.short}</a>
      </li>
    {/each}
  </ul>
  