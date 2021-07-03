<script>
  import { Base64 } from "js-base64";
  export let input = "";
  export let output = "";
</script>

<div class="columns">
  <p class="control">
    <button class="button" on:click={() => (output = Base64.encode(input))}
      >Base64编码</button
    >
  </p>
  <p class="control">
    <button class="button" on:click={() => (output = Base64.decode(input))}
      >Base64解码</button
    >
  </p>

  <p class="control">
    <button
      class="button"
      on:click={() => {
        output = encodeURI(input);
      }}>encodeURI</button
    >
  </p>
  <p class="control">
    <button
      class="button"
      on:click={() => {
        output = decodeURI(input);
      }}>decodeURI</button
    >
  </p>

  <p class="control">
    <button
      class="button"
      on:click={() => {
        output = encodeURIComponent(input);
      }}>encodeURIComponent</button
    >
  </p>
  <p class="control">
    <button
      class="button"
      on:click={() => {
        output = decodeURIComponent(input);
      }}>decodeURIComponent</button
    >
  </p>
</div>

<style>
  .columns {
    display: flex;
    flex-wrap: wrap;
  }
  .columns .control {
    margin: 5px;
  }
</style>
