<script lang="ts">
  import FaPlus from "svelte-icons/fa/FaPlus.svelte";

  const answer = ["Fakie", "kickflip"];

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

  const shuffleSelections = () => {
    for (let i = selectors.length - 1; i > 0; i--) {
      const randomIndex = Math.floor(Math.random() * (i + 1));
      [selectors[i], selectors[randomIndex]] = [
        selectors[randomIndex],
        selectors[i],
      ];
    }
    selectors = [...selectors];
  };

  const handleDeselectAll = () => {
    const answers = document.querySelectorAll(".answers-container > *");
    for (let i = 0; i < answers.length; i++) {
      if (
        answers[i].classList.contains("answer") &&
        !answers[i].id.includes("plus")
      ) {
        answers[i].classList.remove("answer");
        answers[i].classList.add("placeholder");
        answers[i].textContent = placeholders[i];
      }
    }
    selectors = [...stances, ...tricks];
  };

  //checks to see if the answer is correct
  const submitAnswer = () => {
    const answers = document.querySelectorAll(".answers-container > *");
    let answerString = "";
    for (let i = 0; i < answers.length; i++) {
      if (
        answers[i].classList.contains("answer") &&
        !answers[i].id.includes("plus")
      ) {
        answerString += answers[i].textContent?.trim().toLowerCase();
      }
    }

    if (answerString === answer.join("").toLowerCase()) {
      alert("Correct!");
    } else {
      alert("Incorrect!");
    }
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

  <div class="mistakes-container">
    <span>Mistakes Remaining: </span>
    <div class="circle" />
    <div class="circle" />
    <div class="circle" />
    <div class="circle" />
  </div>

  <div class="game-btn-container">
    <button class="game-btn" on:click={shuffleSelections}>Shuffle</button>
    <button class="game-btn" on:click={handleDeselectAll}>Deselect all</button>
    <button class="game-btn" on:click={submitAnswer}>Submit</button>
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

  .game-btn-container {
    display: flex;
    justify-content: center;
    align-items: center;
    padding: 25px;
  }

  .game-btn {
    width: 150px;
    height: 75px;
    background: rgb(90, 114, 155);
    border-radius: 5px;
    display: flex;
    justify-content: center;
    align-items: center;
    margin-right: 25px;
    cursor: pointer;
    color: var(--text-color);
  }

  .mistakes-container {
    display: flex;
    justify-content: center;
    align-items: center;
    padding: 25px;
    color: var(--text-color);
  }

  .circle {
    width: 25px;
    height: 25px;
    border-radius: 50%;
    background: var(--text-color);
    margin: 5px;
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
