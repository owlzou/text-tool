<!-- 正则模块 -->
<script>
  //用相对路径！！
  import Field from "../ui/Field.svelte";
  import CheckBox from "../ui/CheckBox.svelte";
  import { onMount } from "svelte";
  import * as Icon from "../ui/icon";
  export let input = "";
  export let output = "";
  let reg = "";
  let toReplace = "";
  let searchResult = "";
  let searchResultCount = 0;
  let globalSearch = true;
  let capsSearch = false;
  let multilineSearch = false;
  //dom
  let replaceTextArea;

  onMount(async () => load());

  function load() {
    let store = sessionStorage.getItem("regex");

    if (store) {
      const obj = JSON.parse(store);
      reg = obj.reg;
      toReplace = obj.toReplace;
      globalSearch = obj.globalSearch;
      capsSearch = obj.capsSearch;
      multilineSearch = obj.multilineSearch;
    }
  }

  function save() {
    console.log("Reg save");
    sessionStorage.setItem(
      "regex",
      JSON.stringify({
        reg: reg,
        toReplace: toReplace,
        globalSearch: globalSearch,
        capsSearch: capsSearch,
        multilineSearch: multilineSearch,
      })
    );
  }

  function convert() {
    save();
    if (input.length == 0) return;
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
      output = input.replace(regex, toReplace);
      searchResult = input.match(regex) || "";
      searchResultCount = searchResult.length;

      if (searchResult.length > 0) {
        searchResult = searchResult.reduce(
          (pre, cur, index) => `${pre}\n${cur}`
        );
      }
    } catch (e) {
      searchResult = e;
      return;
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
        bind:this={replaceTextArea}
        bind:value={toReplace} />
    </Field>
  </div>
  <div class="column">
    <Field
      label="查找结果 {`（${searchResultCount}）`}">
      <textarea
        class="textarea"
        placeholder="查找结果"
        bind:value={searchResult} />
    </Field>
  </div>
</div>
<Field>
  <button class="button is-primary " on:click={convert}>
    {@html Icon.rotate}
    转换
  </button>
</Field>
