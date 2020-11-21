<!-- 正则模块 -->
<script>
  //用相对路径！！
  import Field from "../ui/Field.svelte";
  import CheckBox from "../ui/CheckBox.svelte";
  export let input = "";
  export let output = "";
  let reg = "";
  let toreplace = "";
  let searchResult = "";
  let globalSearch = false;
  let capsSearch = false;
  let multilineSearch = false;

  function convert() {
    let param = "";
    if (globalSearch) {
      param += "g";
    }
    if (multilineSearch) {
      param += "m";
    }
    if (capsSearch) {
      param += "i";
    }

    let regex;
    try {
      regex = RegExp(reg, param);
    } catch (e) {
      searchResult = e;
      return;
    }

    output = input.replace(regex, toreplace);
    searchResult = input.match(regex);
    if (searchResult != null) {
      searchResult = searchResult.reduce((pre, cur, index) => `${pre}\n${cur}`);
    }
  }
</script>

<div class="columns">
  <div class="column">
    <Field label="正则表达式">
      <input
        class="input"
        type="text"
        placeholder="正则表达式"
        bind:value={reg} />
    </Field>
  </div>
  <div class="column">
    <Field label="选项">
      <CheckBox label="全局搜索" bind:checked={globalSearch} />
      <CheckBox label="不区分大小写" bind:checked={capsSearch} />
      <CheckBox label="多行搜索" bind:checked={multilineSearch} />
    </Field>
  </div>
</div>

<div class="columns">
  <div class="column">
    <Field label="替换字符串">
      <textarea
        class="textarea"
        placeholder="替换字符串"
        bind:value={toreplace} />
    </Field>
  </div>
  <div class="column">
    <Field label="查找结果 - {searchResult.length}">
      <textarea
        class="textarea"
        placeholder="查找结果"
        bind:value={searchResult} />
    </Field>
  </div>
</div>
<Field>
  <button class="button is-primary " on:click={convert}>
    <svg
      style="margin-right:5px"
      xmlns="http://www.w3.org/2000/svg"
      class="icon icon-tabler icon-tabler-rotate-clockwise"
      width="44"
      height="44"
      viewBox="0 0 24 24"
      stroke-width="1.5"
      stroke="#fff"
      fill="none"
      stroke-linecap="round"
      stroke-linejoin="round">
      <path stroke="none" d="M0 0h24v24H0z" fill="none" />
      <path d="M4.05 11a8 8 0 1 1 .5 4m-.5 5v-5h5" fill="none" />
    </svg>转换
  </button>
</Field>
