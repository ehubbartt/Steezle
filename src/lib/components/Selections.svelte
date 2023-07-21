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
  $: shouldHidePlus = placeholders.length >= 4;

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
    element.classList.add("answer");
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

  //if the clicked answer is a placeholder and either the third or fourth answer remove the answer and add the plus button back
  const answerClickHandler = (e: Event, index: number) => {
    const target = e.target as HTMLButtonElement;
    if (index > 1 && target.classList.contains("placeholder")) {
      placeholders.splice(index, 1);
      placeholders = [...placeholders];
    }

    if (target.classList.contains("answer")) {
      if (!target.textContent) return;

      selectors.push(target.textContent);
      selectors = [...selectors];
      target.classList.remove("answer");
      target.classList.add("placeholder");
      target.textContent = placeholders[index];
    }
  };

  const plusClickHandler = () => {
    if (placeholders.length >= 4) {
      return;
    }

    placeholders.push("Trick");
    placeholders = [...placeholders];
  };
</script>

<div class="game-container">
  <div class="answers-container">
    {#each placeholders as answer, index}
      <button
        class="placeholder selector"
        on:click={(e) => answerClickHandler(e, index)}
      >
        {answer}
      </button>
    {/each}

    <button
      class="placeholder trick selector"
      class:hidden={shouldHidePlus}
      id="plus"
      on:click={plusClickHandler}
    >
      <div class="plus-icon">
        <FaPlus />
      </div>
    </button>
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

  .hidden {
    display: none;
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
