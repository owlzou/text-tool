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

  let input = "";
  let output = "";
  let currentTab = "1";

  let tabs = [
    { value: "1", text: "正则" },
    { value: "2", text: "编码" },
    { value: "3", text: "脚本" },
  ];
  let compoents = {
    "1": Reg,
    "2": Encode,
    "3": Editor,
  };

  function cjkCount(word) {
    let count = 0;
    for (let i = 0; i < word.length; i++) {
      if (escape(word[i]).length > 4) {
        count++;
      }
    }
    return count;
  }

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
</script>

<style>
  .container {
    padding: 20px;
  }
</style>

<main>
  <NavBar hasShadow="true">
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
      <Field label="输入文字（{input.length}字，CJK字数{cjkCount(input)}字）">
        <textarea class="textarea" placeholder="输入文字" bind:value={input} />
      </Field>
      <Tabs bind:value={currentTab} options={tabs} />
      <svelte:component this={compoents[currentTab]} {input} bind:output />
      <hr />
      <Field label="输出文字（{output.length}字，CJK字数{cjkCount(output)}字）">
        <textarea
          id="output"
          class="textarea"
          placeholder="输出文字">{output}</textarea>
      </Field>
      <button class="button is-primary" on:click={copy}>
        {@html Icon.copy}复制</button>
    </div>
  </section>
</main>
