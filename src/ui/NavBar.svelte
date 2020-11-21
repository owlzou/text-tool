<script>
  export let color = "white";
  export let hasShadow = false;
  let klass = `navbar is-${color}`;
  let isOpen = false;

  $: if (hasShadow) klass += " has-shadow";
  function open() {
    isOpen = !isOpen;
  }
</script>

<nav class={klass}>
  <div class="navbar-brand">
    <slot name="navbar-brand" />
    <div
      role="button"
      class={isOpen ? "navbar-burger is-active" : "navbar-burger"}
      on:click={open}
      aria-label="menu"
      aria-expanded="false">
      <span aria-hidden="true" />
      <span aria-hidden="true" />
      <span aria-hidden="true" />
    </div>
  </div>

  <div class={isOpen?"navbar-menu is-active":"navbar-menu"} >
    <div class="navbar-start">
      <slot name="navbar-start" />
    </div>

    <div class="navbar-end">
      <slot name="navbar-end" />
    </div>
  </div>
</nav>
