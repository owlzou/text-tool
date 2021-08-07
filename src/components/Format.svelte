<script>
  export let input = "";
  export let output = "";

  /**
   * 格式化表格
   *
   * @param {string} text
   * @param {boolean} [wikitext=false] 是否是 wikitext 格式，否则为 markdown 格式
   * @return {string}
   */
  function formatTable(text, wikitext = false) {
    if (
      text.trim().length == 0 ||
      text.search("\n") < 0 ||
      text.search("|") < 0
    ) {
      return "";
    }

    if (wikitext) {
      text = text.replace(/\[\[(.*)\|(.*)\]\]/g, "[[$1§$2]]");
    } // 替换掉连接中的竖线

    text = text.split("\n");

    // 给每个单元格计数
    let len_arr = []; //number[][]
    let max_col = new Array(text[0].split(/\|/).length - 2).fill(-1); // 记录每列中最长的长度
    const split_line = hasSplitLine(text, wikitext);

    text = text.map((line, index) => {
      line = line.split(/\|/);
      line.pop();
      line.shift();

      let line_len_arr = line.map((cell, i) => {
        line[i] = cell.trim();
        // 不是分割行
        if (split_line && index === 1) {
          return 3;
        } else {
          // 计算竖列最大长度
          let l = count(line[i]);
          max_col[i] = max_col[i] < l ? l : max_col[i];
          return l;
        }
      });

      len_arr.push(line_len_arr);
      return line;
    });

    if (wikitext) {
      text = text
        .map((row) => {
          row = row
            .map((e, i) => e.padEnd(max_col[i] - (count(e) - e.length)))
            .join("|");

          return "|" + row + "|";
        })
        .join("\n")
        .replace(/§/g, "|");
    } else {
      text = text
        .map((row, index) => {
          if (index == 1 && split_line) {
            //分隔行
            row = row
              .map((e, i) => {
                // 查看有无左右对齐
                let n = "".padEnd(max_col[i], "-");

                if (e.match(/^:/)) {
                  n = replaceStr(n, 0, ":");
                }
                if (e.match(/:$/)) {
                  n = replaceStr(n, n.length - 1, ":");
                }

                return ` ${n} `;
              })
              .join("|");
          } else {
            row = row
              .map(
                (e, i) =>
                  " " + e.padEnd(max_col[i] - (count(e) - e.length)) + " "
              )
              .join("|");
          }
          return "|" + row + "|";
        })
        .join("\n");
    }

    return text;
  }

  function replaceStr(str, index, char) {
    return str.substring(0, index) + char + str.substring(index + 1);
  }

  /**
   * 探测是否有markdown表格的分隔线
   *
   * @param {string} text
   * @param {bool} wikitext
   * @return {bool}
   */
  function hasSplitLine(text, wikitext) {
    if (text.length > 2) {
      let cells = text[1].split(/\|/);
      cells = cells.splice(1, cells.length - 2);
      return cells.every((cell) => cell.trim().match(/^:?-+:?$/)) && !wikitext;
    }
    return false;
  }

  /**
   * 计算实际占用的位数（cjk占两格)
   *
   * @param {string} word
   * @return {number}
   */
  function count(word) {
    let count = 0;
    for (let i = 0; i < word.length; i++) {
      if (escape(word[i]).length > 4) {
        count += 2;
      } else {
        count++;
      }
    }
    return count;
  }
</script>

<div class="columns">
  <p class="control">
    <button class="button" on:click={() => (output = formatTable(input, false))}
      >格式化 Markdown 表格</button
    >
  </p>

  <p class="control">
    <button class="button" on:click={() => (output = formatTable(input, true))}
      >格式化 TiddlyWiki 表格</button
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