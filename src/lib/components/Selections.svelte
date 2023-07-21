<script>
  import FaPlus from "svelte-icons/fa/FaPlus.svelte";
</script>

<div class="game-container">
  <div class="answers-container">
    <div class="answer stance selector">Stance</div>
    <div class="answer trick selector">Trick</div>
    <div class="answer trick selector">
      <div class="plus-icon">
        <FaPlus />
      </div>
    </div>
  </div>

  <div class="selectors-container">
    <div class="selector">Regular</div>
    <div class="selector">Fakie</div>
    <div class="selector">Nollie</div>
    <div class="selector">Switch</div>
    <div class="selector">kickflip</div>
    <div class="selector">ollie</div>
    <div class="selector">laser flip</div>
    <div class="selector">late flip</div>
    <div class="selector">kickflip</div>
    <div class="selector">ollie</div>
    <div class="selector">late heelflip</div>
    <div class="selector">impossible</div>
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

  .answer {
    background: var(--background-color);
    color: var(--text-color);
    border: 1px solid var(--text-color);
    border-radius: 5px;
  }

  .plus-icon {
    width: 15px;
  }
</style>
