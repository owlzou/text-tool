<script>
  import "bulma/css/bulma.css";
  import "bulma-checkradio/dist/css/bulma-checkradio.min.css";
  //
  import { Field, Tabs, NavBar, Footer } from "./ui/index.svelte";
  import * as Icon from "./ui/icon";
  //
  import {
    Reg,
    Encode,
    Editor,
    State,
    Format,
  } from "./components/index.svelte";

  let input = "";
  let output = "";
  let currentTab = "1";

  let tabs = [
    { value: "1", text: "正则" },
    { value: "2", text: "编码" },
    { value: "3", text: "脚本" },
    { value: "4", text: "格式" },
    { value: "5", text: "统计" },
  ];
  let compoents = {
    "1": Reg,
    "2": Encode,
    "3": Editor,
    "4": Format,
    "5": State,
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

  // PWA 注册 Worker
  if ("serviceWorker" in navigator) {
    navigator.serviceWorker.register("../serviceworker.js",{scope:"../"}).catch((e) => {
      console.log("ServiceWorker registration failed", e);
    });
  }
</script>

<main>
  <NavBar hasShadow="true">
    <div slot="navbar-brand" class="navbar-item">
      <img src="./images/apple-touch-icon.png" alt="logo" />&nbsp;
      <h2>文字处理工具</h2>
    </div>
    <div slot="navbar-end" class="navbar-item">
      <div class="field is-grouped">
        <div class="control">
          <a
            class="button is-link is-light"
            href="https://github.com/owlzou/text-tool"
            target="_blank"
          >
            {@html Icon.github}
            <span>GitHub</span></a
          >
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
        <textarea id="output" class="textarea" placeholder="输出文字"
          >{output}</textarea
        >
      </Field>
      <button class="button is-primary" on:click={copy}>
        {@html Icon.copy}复制</button
      >
      <button class="button is-light" on:click={copyToInput}
        >{@html Icon.arrowNarrowUp}复制到输入框</button
      >
    </div>
  </section>
  <Footer>
    <div>文字处理工具✒️2020-{new Date().getFullYear()}</div>
  </Footer>
</main>

<style>
  .container {
    padding: 20px;
  }
  :global(.button .icon-tabler) {
    margin-right: 5px !important;
  }
  #output {
    font-family: "sarasa mono sc", "lxgw wenkai mono", "Courier New", Courier,
      monospace;
  }
</style>
