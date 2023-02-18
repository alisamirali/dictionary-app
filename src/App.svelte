<script>
  import "../public/global.css";

  import Loader from "./Loader.svelte";
  import WordData from "./WordData.svelte";

  let word = "";
  let loading = false;
  let wordData = null;

  async function searchWord() {
    if (word.length === 0) {
      return;
    }

    loading = true;
    wordData = null;

    try {
      let result = await fetch(
        "https://api.dictionaryapi.dev/api/v2/entries/en/" + word
      );

      let data = await result.json();

      if (Array.isArray(data)) {
        wordData = data[0];
      } else {
        wordData = "No Result Found!";
      }

      loading = false;
    } catch (err) {
      loading = false;
    }
  }
</script>

<main>
  <div class="header">Dictionary App</div>

  <div class="center">
    <div class="form">
      <div class="form__group">
        <!-- svelte-ignore a11y-label-has-associated-control -->
        <label>Search Word</label>
        <input type="text" placeholder="Type word here..." bind:value={word} />
      </div>

      <div class="form__group">
        <button on:click={searchWord}>Search</button>
      </div>
    </div>

    {#if loading === true || wordData !== null}
      <div class="result">
        {#if wordData !== null && typeof wordData !== "string"}
          <WordData {wordData} />
        {:else if wordData === null && loading === true}
          <Loader />
        {:else}
          <p class="padding">No Result Found</p>
        {/if}
      </div>
    {/if}
  </div>
</main>

<style>
  .header {
    width: 100%;
    padding: 20px;
    background: #feb824;
    color: #fff;
    font-size: 20px;
    font-weight: 600;
    text-align: center;
  }

  .center {
    margin: 40px auto;
    width: 95%;
    max-width: 550px;
    background: #fff;
  }

  .form {
    background: #fff;
    padding: 20px;
    border: 1px solid #f5f5f5;
    box-shadow: 0px 0px 5px 2px rgba(0, 0, 0, 0.05);
    margin: 20px 0px;
  }

  .form .form__group {
    margin: 10px;
    position: relative;
  }

  .form .form__group label {
    display: block;
    font-size: 18px;
    margin-bottom: 8px;
  }

  .form .form__group input {
    width: 100%;
    padding: 10px;
    font-size: 15px;
    border: 1px solid #bbb;
  }

  .form .form__group button {
    padding: 10px 20px;
    background: #feb824;
    color: #fff;
    font-size: 15px;
    font-weight: 500;
    border: none;
    outline: none;
    cursor: pointer;
    transition: all 0.3s ease;
  }

  .form .form__group button:hover {
    background: #111;
    color: #fff;
  }

  .result {
    background: #fff;
    border: 1px solid #f5f5f5;
    box-shadow: 0px 0px 5px 2px rgba(0, 0, 0, 0.05);
  }

  .result .padding {
    padding: 20px;
  }
</style>
