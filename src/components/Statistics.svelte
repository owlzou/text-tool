<script>
  import Field from "../ui/Field.svelte";
  import Table from "../ui/Table.svelte";
  export let input = "";
  export let output = "";


  function cjkCount(word) {
   let len = 0;
    for (var i = 0; i < word.length; i++) {
      if (word.charAt(i).match(/[\u4e00-\u9fa5]/)|| word.charAt(i).match(/[\u9FA6-\u9fcb]/)) {
        len += 1
      }
    }
    return len
  }

  function getData(word) {
    return [
      {
        type: "总长度",
        count: word.length,
      },
      { type: "CJK文字数量", count: cjkCount(word) },
      {
        type: "行数",
        count: (word.match(/\n/g) && word.match(/\n/g).length + 1) || 0,
      },
      {
        type: "非空格字符数",
        count: word.replace(/\s/g, "").length || 0,
      },
    ];
  }

  let col = [
    { label: "类别", prop: "type" },
    { label: "数量", prop: "count" },
  ];
</script>


  <div class="columns">
    <div class="column">
      <Field label="输入文字">
        <Table data={getData(input)} columns={col} />
      </Field>
    </div>
    <div class="column">
      <Field label="输出文字">
        <Table data={getData(output)} columns={col} />
      </Field>
    </div>
  </div>

