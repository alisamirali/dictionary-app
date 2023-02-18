<script>
  export let wordData = null;

  function playAudio(src) {
    let audio = new Audio(src);

    audio.play();
  }
</script>

<div class="word__data">
  {#if wordData.phonetics}
    <div class="audios">
      {#each wordData.phonetics as data}
        <div class="audio">
          <p>
            {data.text}
            <!-- svelte-ignore a11y-click-events-have-key-events -->
            <i
              class="bx bx-play-circle"
              on:click={() => playAudio(data.audio)}
            />
          </p>
        </div>
      {/each}
    </div>
  {/if}

  {#each wordData.meanings as meaning}
    <div class="meaning">
      <div class="part__of__speech">
        &bull;
        {meaning.partOfSpeech}
      </div>

      <div class="definitions">
        {#each meaning.definitions as data}
          <div class="definition">
            <p>Definition: <b>{data.definition}</b></p>

            {#if data.example}
              <p>Example: <i>{data.example}</i></p>
            {/if}
          </div>
        {/each}
      </div>
    </div>
  {/each}
</div>

<style>
  .word__data .audios {
    border-bottom: 1px solid #ddd;
  }

  .word__data .audios .audio {
    padding: 8px 20px;
  }

  .word__data .audios .audio i {
    cursor: pointer;
    font-size: 17px;
  }

  .word__data .meaning {
    padding: 8px 20px;
    border-bottom: 1px solid #ddd;
  }

  .word__data > div:last-child {
    border-bottom: none;
  }

  .word__data .meaning .part__of__speech {
    font-size: 17px;
    font-weight: 600;
    margin: 10px 0 0;
  }

  .word__data .meaning .definitions .definition {
    padding: 8px 0px;
  }

  .word__data .meaning .definitions .definition p {
    margin: 5px 0px;
  }
</style>
