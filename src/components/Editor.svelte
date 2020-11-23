<script>
  import "codemirror/theme/duotone-light.css";
  import "codemirror/lib/codemirror.css";
  import "codemirror/mode/javascript/javascript";
  import CodeMirror from "codemirror";
  //
  import Field from "../ui/Field.svelte";
  import Message from "../ui/Message.svelte";
  import * as Icon from "../ui/icon";
  import { onMount } from "svelte";

  export let input = "";
  export let output = "";
  const func = `output=input+"Output"`;
  let iframe;
  let cm;
  let code;

  onMount(async () => {
    code = CodeMirror.fromTextArea(cm, {
      mode: "javascript",
      theme: "duotone-light",
      lineNumbers: true,
    });
  });

  function run() {
    var input_post = input.replace(/([{}`$\\])/g, "\\$1");
    var idoc = iframe.contentDocument || iframe.contentWindow.document;

    var span = idoc.createElement("span");
    span.id = "span";
    idoc.body.appendChild(span);

    var ele = idoc.createElement("script");
    ele.text = `
        var input=\`${input_post}\`
        var output = ""
        ${code.getValue()}
        document.getElementById("span").text= output
        `;

    idoc.body.appendChild(ele);

    //返回output
    output = idoc.getElementById("span").text;
    idoc.getElementById("span").text = "";

    //去掉脚本
    idoc.body.innerHTML = "";
  }
</script>

<Message type="is-primary" closeable="true">
  <p slot="message-header">提示</p>
  <div slot="message-body">
    变量<code>Input</code>指代输入文字，变量<code>Output</code>指代输出文字。
  </div>
</Message>
<iframe bind:this={iframe} style="display:none" title="vis" />

<Field label="输入脚本">
  <textarea class="textarea" bind:this={cm} value={func} />
</Field>
<Field>
  <button class="button is-primary " on:click={run}>
    {@html Icon.play}
    转换
  </button>
</Field>
