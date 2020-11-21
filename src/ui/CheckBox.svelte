<script>
  export let label;
  export let checked = false;

  function onClick() {
    checked = !checked;
  }
</script>

<input class="is-checkradio" type="checkbox" name={label} {checked} />
<label for={label} on:click={onClick}>{label}</label>
