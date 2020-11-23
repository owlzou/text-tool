<script>
  import Field from "../ui/Field.svelte";
  import { Base64 } from "js-base64";
  export let input = "";
  export let output = "";

</script>


  <div class="columns">
    <div class="column">
      <Field group="true">
        <p class="control">
          <button
            class="button"
            on:click={() => (output = Base64.encode(input))}>Base64编码</button>
        </p>
        <p class="control">
          <button
            class="button"
            on:click={() => (output = Base64.decode(input))}>Base64解码</button>
        </p>
      </Field>
    </div>

    <div class="column">
      <Field group="true">
        <p class="control">
          <button
            class="button"
            on:click={() => {
              output = encodeURI(input);
            }}>encodeURI</button>
        </p>
        <p class="control">
          <button
            class="button"
            on:click={() => {
              output = decodeURI(input);
            }}>decodeURI</button>
        </p>
      </Field>
    </div>

    <div class="column">
      <Field group="true">
        <p class="control">
          <button
            class="button"
            on:click={() => {
              output = encodeURIComponent(input);
            }}>encodeURIComponent</button>
        </p>
        <p class="control">
          <button
            class="button"
            on:click={() => {
              output = decodeURIComponent(input);
            }}>decodeURIComponent</button>
        </p>
      </Field>
    </div>
  </div>

