<script>
  import "bulma/css/bulma.css";
  import "bulma-checkradio/dist/css/bulma-checkradio.min.css";
  //
  import Field from "./ui/Field.svelte";
  import Tabs from "./ui/Tabs.svelte";
  import NavBar from "./ui/NavBar.svelte";
  import * as Icon from "./ui/icon";
  //
  import Reg from "./components/Reg.svelte";
  import Encode from "./components/Encode.svelte";
  import Editor from "./components/Editor.svelte";
  import State from "./components/Statistics.svelte";

  let input = "";
  let output = "";
  let currentTab = "1";

  let tabs = [
    { value: "1", text: "正则" },
    { value: "2", text: "编码" },
    { value: "3", text: "脚本" },
    { value: "4", text: "统计" },
  ];
  let compoents = {
    "1": Reg,
    "2": Encode,
    "3": Editor,
    "4": State,
  };

  function copy() {
    let obj = document.getElementById("output");
    obj.focus();
    obj.select();
    try {
      if (document.execCommand("copy", false, null)) {
      } else {
        console.log("不支持 execCommand");
      }
    } catch (e) {
      console.log(e);
    }
  }

  function copyToInput() {
    input = output;
  }
</script>

<style>
  .container {
    padding: 20px;
  }
  :global(.button .icon-tabler) {
    margin-right: 5px !important;
  }
</style>

<main>
  <NavBar hasShadow="true">
    <div slot="navbar-brand" class="navbar-item">
      <h2>文字处理工具</h2>
    </div>
    <div slot="navbar-end" class="navbar-item">
      <div class="field is-grouped">
        <div class="control">
          <a
            class="button is-link is-light"
            href="https://github.com/owlzou/text-tool"
            target="_blank">
            {@html Icon.github}
            <span>GitHub</span></a>
        </div>
      </div>
    </div>
  </NavBar>
  <!--  -->
  <section>
    <div class="container">
      <Field label="输入文字">
        <textarea class="textarea" placeholder="输入文字" bind:value={input} />
      </Field>
      <Tabs bind:value={currentTab} options={tabs} />
      <svelte:component this={compoents[currentTab]} {input} bind:output />
      <hr />
      <Field label="输出文字">
        <textarea
          id="output"
          class="textarea"
          placeholder="输出文字">{output}</textarea>
      </Field>
      <button class="button is-primary" on:click={copy}>
        {@html Icon.copy}复制</button>
      <button
        class="button is-light"
        on:click={copyToInput}>{@html Icon.arrowNarrowUp}复制到输入框</button>
    </div>
  </section>
</main>
