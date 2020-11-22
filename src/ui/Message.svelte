<script>
  export let closeable = false;
  export let type = "";
  let visible = true;
</script>

<article class="message {type}" style="display:{visible ? 'block' : 'none'}">
  <div class="message-header">
    <slot name="message-header" />
    {#if closeable}
      <button
        class="delete"
        aria-label="delete"
        on:click={() => (visible = !visible)} />
    {/if}
  </div>
  <div class="message-body">
    <slot name="message-body" />
  </div>
</article>
