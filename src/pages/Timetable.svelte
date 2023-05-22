<script>
  export let timetable;
</script>

<table class="striped centered teal lighten-1 responsive-table">
  <thead>
    <tr>
      <th></th>
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
            <td></td>
          {/each}
      </tr>
    {/each}
  </tbody>
</table>
