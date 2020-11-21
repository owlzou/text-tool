<script>
  export let value;
  export let options;
  function onChange(val){
     value = val
  }
</script>

<div class="tabs is-boxed">
  <ul>
    {#each options as tab}
      {#if tab.value==value}
        <li class="is-active">
          <a href="#" on:click={onChange(tab.value)}>
            <span>{tab.text}</span>
          </a>
        </li>
      {:else}
        <li>
          <a href="#" on:click={onChange(tab.value)}>
            <span>{tab.text}</span>
          </a>
        </li>
      {/if}
    {/each}
  </ul>
</div>
