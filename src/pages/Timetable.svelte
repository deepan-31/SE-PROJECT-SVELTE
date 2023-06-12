<script>
  import { createEventDispatcher } from "svelte";
  import { saveAs } from "file-saver";
  import Papa from "papaparse";
  const dispatch = createEventDispatcher();

  export let timetable;

  function handleCancel() {
    dispatch("cancel");
  }
  async function exportTable() {
    const table = document.getElementById("tblExport1");
    const csv = Papa.unparse(tableToData(table));
    const blob = new Blob([csv], { type: "text/csv;charset=utf-8" });
    saveAs(blob, "timetable.csv");
  }

  function tableToData(table) {
    const rows = Array.from(table.getElementsByTagName("tr"));
    const data = [];

    for (let i = 0; i < rows.length; i++) {
      const row = rows[i];
      const rowData = [];

      const cells = row.getElementsByTagName("td");
      for (let j = 0; j < cells.length; j++) {
        rowData.push(cells[j].innerText);
      }

      data.push(rowData);
    }

    return data;
  }
</script>

<div>
  <button class="btn red" on:click={handleCancel}>Cancel</button>
  <div class="card z-depth-3">
    <table class="striped yellow darken-4" id="tblExport1">
      <thead>
        <tr>
          <th />
          <th>Slot 1</th>
          <th>Slot 2</th>
          <th>Slot 3</th>
          <th>Slot 4</th>
          <th>Slot 5</th>
          <th>Slot 6</th>
          <th>Slot 7</th>
          <th>Slot 8</th>
        </tr>
      </thead>
      <tbody>
        {#each ["monday", "tuesday", "wednesday", "thursday", "friday", "saturday"] as day}
          <tr>
            <th>{day}</th>
            {#each [1, 2, 3, 4, 5, 6, 7, 8] as slot}
              {#each timetable.filter((t) => t.day === day && t.slot === slot) as item}
                <td>{item.course_code}</td>
              {/each}
            {:else}
              <td />
            {/each}
          </tr>
        {/each}
      </tbody>
    </table>
  </div>
  <div>
    <button
      id="btnExport"
      class="btn waves-effect waves-light light-blue"
      on:click={exportTable}>Export to csv</button
    >
  </div>
</div>
