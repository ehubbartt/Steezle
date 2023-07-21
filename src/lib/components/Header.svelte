<script>
  import MdHelpOutline from "svelte-icons/md/MdHelpOutline.svelte";
</script>

<div class="header-container">
  <div class="header">
    <div class="logo">
      <img src="/images/logo.png" alt="logo" />
    </div>
    <div class="title">Steezle</div>
    <div class="menu">
      <div class="help-icon">
        <MdHelpOutline />
      </div>
    </div>
  </div>
</div>

<style>
  .header-container {
    display: flex;
    flex-direction: row;
    justify-content: center;
    width: 100vw;
  }

  .header {
    align-items: center;
    display: grid;
    flex-grow: 1;
    font-size: 24px;
    font-weight: 500;
    grid-template-columns: repeat(3, 1fr);
    max-width: var(--max-width);
    padding: 16px;
    color: var(--text-color);
  }

  .title {
    text-align: center;
    font-weight: bold;
    font-size: 32px;
  }

  .menu {
    display: flex;
    align-items: center;
    justify-content: flex-end;
  }

  .help-icon {
    height: 32px;
  }

  .logo {
    display: flex;
    align-items: center;
  }

  .logo > img {
    height: 32px;
    filter: invert(100%);
  }
</style>
