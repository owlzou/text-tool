<script>
  export let data = [];
  export let columns = [];//[{prop="",label=""}]
</script>

<table class="table">
  <thead>
    <tr>
      {#each columns as col}
        <th>{col.label}</th>
      {/each}
    </tr>
  </thead>
  <tbody>
    {#each data as ele}
      <tr>
        {#each columns as col}
          <td>{ele[col.prop]}</td>
        {/each}
      </tr>
    {/each}
  </tbody>
</table>
