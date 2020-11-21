<script>
  import "bulma/css/bulma.css";
  import "bulma-checkradio/dist/css/bulma-checkradio.min.css";
  //
  import Field from "./ui/Field.svelte";
  import Tabs from "./ui/Tabs.svelte";
  import NavBar from "./ui/NavBar.svelte";
  //
  import Reg from "./components/Reg.svelte";
  import Encode from "./components/Encode.svelte";

  let input = "";
  let output = "";
  let currentTab = "1";

  let tabs = [
    { value: "1", text: "正则" },
    { value: "2", text: "编码" },
  ];
  let compoents = {
    "1": Reg,
    "2": Encode,
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
            <svg
              xmlns="http://www.w3.org/2000/svg"
              class="icon icon-tabler icon-tabler-brand-github"
              width="44"
              height="44"
              viewBox="0 0 24 24"
              stroke-width="1.5"
              stroke="#2160c4"
              fill="none"
              stroke-linecap="round"
              stroke-linejoin="round">
              <path stroke="none" d="M0 0h24v24H0z" fill="none" />
              <path
                d="M9 19c-4.3 1.4 -4.3 -2.5 -6 -3m12 5v-3.5c0 -1 .1 -1.4 -.5 -2c2.8 -.3 5.5 -1.4 5.5 -6a4.6 4.6 0 0 0 -1.3 -3.2a4.2 4.2 0 0 0 -.1 -3.2s-1.1 -.3 -3.5 1.3a12.3 12.3 0 0 0 -6.2 0c-2.4 -1.6 -3.5 -1.3 -3.5 -1.3a4.2 4.2 0 0 0 -.1 3.2a4.6 4.6 0 0 0 -1.3 3.2c0 4.6 2.7 5.7 5.5 6c-.6 .6 -.6 1.2 -.5 2v3.5" />
            </svg>
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
      <hr />
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
        <svg
          style="margin-right:5px"
          xmlns="http://www.w3.org/2000/svg"
          class="icon icon-tabler icon-tabler-copy"
          width="44"
          height="44"
          viewBox="0 0 24 24"
          stroke-width="1.5"
          stroke="#fff"
          fill="none"
          stroke-linecap="round"
          stroke-linejoin="round">
          <path stroke="none" d="M0 0h24v24H0z" fill="none" />
          <rect x="8" y="8" width="12" height="12" rx="2" />
          <path
            d="M16 8v-2a2 2 0 0 0 -2 -2h-8a2 2 0 0 0 -2 2v8a2 2 0 0 0 2 2h2" />
        </svg>复制</button>
    </div>
  </section>
</main>
