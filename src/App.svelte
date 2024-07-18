<script lang="ts">
  type Pattern = {
    name: string;
    backgroundImage: string;
  };

  const patterns: { [key: string]: Pattern } = {
    "pattern-1": {
      name: "Horizontal Lines",
      backgroundImage:
        "linear-gradient(0deg, rgb(0, 0, 0) 50%, rgb(255, 255, 255) 50%)",
    },
    "pattern-2": {
      name: "Vertical Lines",
      backgroundImage:
        "linear-gradient(to right, rgb(0,0,0), rgb(0,0,0) 5px, rgb(255,255,255) 5px, rgb(255,255,255))",
    },
  };

  function handlePatternButtonClick(
    event: Event,
    patternKey: keyof typeof patterns
  ) {
    // set the app background to the same pattern
    // as the button that was clicked
    console.log(patternKey);
    const pattern = patterns[patternKey];
    console.log(pattern);

    // document.body.style.backgroundImage = pattern.backgroundImage;
    const patternList = document.querySelector<HTMLDivElement>(".pattern-list");
    if (!patternList) return;
    patternList.style.backgroundImage = pattern.backgroundImage;
    patternList.style.backgroundSize = "10px 10px";
  }
</script>

<main>
  <h1>Pattern Bae</h1>
  <div class="pattern-list">
    {#each Object.keys(patterns) as pattern}
      <button
        class="pattern-button"
        on:click={(event) => handlePatternButtonClick(event, pattern)}
      >
        <div class="pattern-box">
          <div
            class="pattern"
            style="background-image: {patterns[pattern].backgroundImage}"
          >
            <h2 class="pattern-name">{patterns[pattern].name}</h2>
          </div>
        </div>
      </button>
    {/each}
  </div>
</main>

<style>
  .pattern-box {
    display: flex;
    flex-direction: column;
  }
  .pattern {
    padding: 10px;
    margin: 10px 0;
    background-size: 10px 10px;
    color: white;
    text-align: center;
  }
  .pattern-button {
    width: 100%;
    display: flex;
    padding: 0px;
    background: none;
    border: none;
    cursor: pointer;
    transition: transform 0.2s ease-in-out;
  }
  .pattern-name {
    margin: 0;
    font-size: small;
    background-color: white;
    color: black;
  }
</style>
