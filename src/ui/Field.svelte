<script>
  export let label = "";
  export let group = false;
</script>

<div class="field" class:is-grouped={group}>
  {#if label}<label for={label} class="label">{label}</label>{/if}
  {#if group}
    <slot />
  {:else}
    <p class="control">
      <slot />
    </p>
  {/if}
</div>
