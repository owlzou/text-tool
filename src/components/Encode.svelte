<script>
  import Field from "../ui/Field.svelte";
  import { Base64 } from "js-base64";
  export let input = "";
  export let output = "";

  function base64encode() {
    output = Base64.encode(input);
  }

  function base64decode() {
    output = Base64.decode(input);
  }

</script>

<div class="columns">
  <div class="column">
    <Field>
      <button class="button" on:click={base64encode}>Base64编码</button>
    </Field>
  </div>
  <div class="column">
    <Field>
      <button class="button" on:click={base64decode}>Base64解码</button>
    </Field>
  </div>
</div>
