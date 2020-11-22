<script>
  export let value;
  export let options;
  function onChange(val) {
    value = val;
  }
</script>

<div class="tabs is-boxed">
  <ul>
    {#each options as tab}
      <li class:is-active={tab.value == value}>
        <a href="#" on:click={onChange(tab.value)}> <span>{tab.text}</span> </a>
      </li>
    {/each}
  </ul>
</div>
