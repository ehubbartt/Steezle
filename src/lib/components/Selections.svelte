<script lang="ts">
  import FaPlus from "svelte-icons/fa/FaPlus.svelte";

  let stances = ["Regular", "Fakie", "Nollie", "Switch"];
  let tricks = [
    "kickflip",
    "ollie",
    "laser flip",
    "late flip",
    "kickflip",
    "ollie",
    "late heelflip",
    "impossible",
  ];
  let selectors = [...stances, ...tricks];

  let placeholders = ["Stance", "Trick"];

  //return the first answer that has the placeholder class
  const getFirstAnswer = (): Element | null => {
    const answers = document.querySelectorAll(".answers-container > *");
    for (let i = 0; i < answers.length; i++) {
      if (
        answers[i].classList.contains("placeholder") &&
        !answers[i].id.includes("plus")
      ) {
        return answers[i];
      }
    }
    return null;
  };

  //remove placeholder class from given element and change text to given text
  const setAnswer = (answer: string, element: Element) => {
    element.classList.remove("placeholder");
    element.textContent = answer;
  };

  const selectorClickHandler = (e: Event) => {
    const clickedSelector = e.target as HTMLButtonElement;

    const firstAnswer = getFirstAnswer();

    if (!firstAnswer) {
      return;
    }
    if (!clickedSelector.textContent) {
      return;
    }

    setAnswer(clickedSelector.textContent, firstAnswer);

    const clickedSelectorIndex = selectors.indexOf(
      clickedSelector.textContent.trim()
    );

    selectors.splice(clickedSelectorIndex, 1);
    selectors = [...selectors];
  };
</script>

<div class="game-container">
  <div class="answers-container">
    {#each placeholders as answer}
      <button class="placeholder selector">
        {answer}
      </button>
    {/each}

    <div class="placeholder trick selector" id="plus">
      <div class="plus-icon">
        <FaPlus />
      </div>
    </div>
  </div>

  <div class="selectors-container">
    {#each selectors as selector}
      <button class="selector" on:click={selectorClickHandler}>
        {selector}
      </button>
    {/each}
  </div>
</div>

<style>
  .game-container {
    display: flex;
    flex-direction: column;
    justify-content: center;
    max-width: var(--max-width);
    margin: auto;
  }

  .answers-container {
    display: flex;
    justify-content: center;
    align-items: center;
    padding: 25px;
  }

  .selectors-container {
    display: flex;
    flex-direction: row;
    justify-content: center;
    align-items: center;
    flex-wrap: wrap;
    border-bottom: 1px solid var(--text-color);
    border-top: 1px solid var(--text-color);
  }

  .selector {
    width: 150px;
    height: 75px;
    background: whitesmoke;
    border-radius: 5px;
    display: flex;
    justify-content: center;
    align-items: center;
    margin: 25px;
    cursor: pointer;
    color: var(--background-color);
  }

  .placeholder {
    background: var(--background-color);
    color: var(--text-color);
    border: 1px solid var(--text-color);
    border-radius: 5px;
  }

  .plus-icon {
    width: 15px;
  }

  button {
    background: none;
    color: inherit;
    border: none;
    padding: 0;
    font: inherit;
    cursor: pointer;
    outline: inherit;
  }
</style>
