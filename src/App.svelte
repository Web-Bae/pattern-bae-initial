<script lang="ts">
  import { onMount } from "svelte";

  let selectedElement: AnyElement | null;
  onMount(async () => {
    webflow.subscribe("selectedelement", setSelectedElement);
  });

  function setSelectedElement(element: AnyElement | null) {
    selectedElement = element;
  }

  async function createWebflowElement() {
    if (!selectedElement) return;

    const newDiv = await selectedElement.after(webflow.elementPresets.DivBlock);

    console.log(`${JSON.stringify(newDiv)}`);
  }

  type Pattern = {
    name: string;
    backgroundImage: string;
  };

  const patterns: { [key: string]: Pattern } = {
    "pattern-1": {
      name: "Horizontal",
      backgroundImage:
        "linear-gradient(0deg, rgb(0, 0, 0) 50%, rgb(255, 255, 255) 50%)",
    },
    "pattern-2": {
      name: "Vertical",
      backgroundImage:
        "linear-gradient(to right, rgb(0,0,0), rgb(0,0,0) 5px, rgb(255,255,255) 5px, rgb(255,255,255))",
    },
    "pattern-3": {
      name: "Diagonal",
      backgroundImage:
        "linear-gradient(45deg, rgb(0, 0, 0) 25%, rgb(255, 255, 255) 25%, rgb(255, 255, 255) 50%, rgb(0, 0, 0) 50%, rgb(0, 0, 0) 75%, rgb(255, 255, 255) 75%, rgb(255, 255, 255))",
    },
  };

  function handlePatternButtonClick(
    event: Event,
    patternKey: keyof typeof patterns
  ) {
    const pattern = patterns[patternKey];

    const patternSection =
      document.querySelector<HTMLDivElement>(".section-pattern");
    if (!patternSection) return;
    patternSection.style.backgroundImage = pattern.backgroundImage;
    patternSection.style.backgroundSize = "10px 10px";
  }
</script>

<main>
  <section>
    <div class="container">
      {#if selectedElement}
        <p>{selectedElement.type}</p>
        <button on:click={createWebflowElement}>Create Div</button>
      {/if}
    </div>
  </section>
  <section class="section-pattern">
    <div class="container">
      <div class="pattern-list">
        {#each Object.keys(patterns) as pattern}
          <button
            class="pattern-button"
            on:click={(event) => handlePatternButtonClick(event, pattern)}
          >
            <div
              class="pattern-box"
              style="background-image: {patterns[pattern].backgroundImage}"
            >
              <h2 class="pattern-name">{patterns[pattern].name}</h2>
            </div>
          </button>
        {/each}
      </div>
    </div>
  </section>
</main>

<style>
  .container {
    padding: 1rem;
  }
  .pattern-list {
    display: grid;
    grid-template-columns: 1fr 1fr 1fr;
    gap: 1rem;
  }
  .pattern-button {
    width: 100%;
    display: flex;
    padding: 0px;
    background: none;
    border: none;
    cursor: pointer;
    transition: transform 0.2s ease-in-out;
    border-radius: 4px;
    overflow: hidden;
  }
  .pattern-box {
    width: 100%;
    display: flex;
    flex-direction: column;
    justify-content: end;
    align-items: center;
    aspect-ratio: 1 / 1;
    background-size: 10px 10px;
    padding-bottom: 0.5rem;
  }
  .pattern-name {
    padding: 0.25rem;
    font-size: small;
    background-color: white;
    color: black;
    border-radius: 4px;
  }
</style>
