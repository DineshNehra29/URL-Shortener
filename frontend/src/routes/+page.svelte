<script lang="ts">
  import axios from 'axios';
  import { onMount } from 'svelte';

  let url: string = '';
  let shortUrl: string = '';
  let loading: boolean = false;
  let error: string = '';
  let previousLinks: { original: string; short: string }[] = [];

  const shortenURL = async () => {
    if (!url) {
      error = 'Please enter a URL';
      return;
    }

    loading = true;
    error = '';
    try {
      const response = await axios.post('http://localhost:3001/shorten', {
        url: url,
      });
      shortUrl = response.data.short_url;
      previousLinks = [{ original: url, short: shortUrl }, ...previousLinks];
      url = '';
    } catch (err) {
      error = 'Failed to shorten URL. Please try again.';
    } finally {
      loading = false;
    }
  };
</script>

<style>
  main {
    font-family: 'Helvetica Neue', sans-serif;
    text-align: center;
    padding: 50px;
    background: linear-gradient(135deg, #e09, #d0e);
    color: #333;
    min-height: 100vh;
  }

  input[type='text'] {
    padding: 15px;
    width: 60%;
    margin: 20px 0;
    border: 1px solid #ddd;
    border-radius: 25px;
    box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
    transition: all 0.3s ease;
  }

  input[type='text']:focus {
    outline: none;
    box-shadow: 0 4px 12px rgba(0, 0, 0, 0.2);
    border-color: #007bff;
  }

  button {
    padding: 10px 30px;
    background-color: #007bff;
    color: white;
    border: none;
    border-radius: 25px;
    cursor: pointer;
    box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
    transition: background-color 0.3s ease;
  }

  button:hover {
    background-color: #0056b3;
  }

  button:disabled {
    background-color: #cccccc;
    cursor: not-allowed;
  }

  button:disabled {
    background-color: #cccccc;
    cursor: not-allowed;
  }

  .error {
    color: #ff4d4d;
    background: #ffe6e6;
    padding: 10px;
    border-radius: 10px;
    width: 60%;
    margin: 10px auto;
  }

  .link-list {
    text-align: left;
    margin: 20px auto;
    width: 60%;
    background: white;
    padding: 20px;
    border-radius: 15px;
    box-shadow: 0 8px 16px rgba(0, 0, 0, 0.2);
    transition: transform 0.3s ease;
  }

  .link-list:hover {
    transform: translateY(-5px);
  }

  .link-list ul {
    list-style: none;
    padding: 0;
  }

  .link-list li {
    padding: 10px 0;
    border-bottom: 1px solid #f4f4f9;
  }

  .link-list a {
    color: #007bff;
    text-decoration: none;
  }

  .link-list a:hover {
    text-decoration: underline;
  }
</style>

<main>
  <h1 style="font-size: 3em; color: #4A00E0; text-shadow: 2px 2px #FF8C00;">✂️ URL Shortener ✨</h1>

  <input type="text" bind:value={url} placeholder="Enter a URL..." />
  <button on:click={shortenURL} disabled={loading}>Shorten</button>

  {#if loading}
    <p>Shortening...</p>
  {/if}

  {#if error}
    <p class="error">{error}</p>
  {/if}

  {#if shortUrl}
    <p>Shortened URL: <a href={shortUrl} target="_blank">{shortUrl}</a></p>
  {/if}

  {#if previousLinks.length > 0}
    <div class="link-list">
      <h2>Previous Links</h2>
      <ul>
        {#each previousLinks as link}
          <li>
            <a href={link.original} target="_blank">{link.original}</a> →
            <a href={link.short} target="_blank">{link.short}</a>
          </li>
        {/each}
      </ul>
    </div>
  {/if}
</main>
