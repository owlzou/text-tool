<script>
  export let label = "";
</script>

<div class="field">
  {#if label}<label for={label} class="label">{label}</label>{/if}
  <div class="control">
    <slot />
  </div>
</div>
