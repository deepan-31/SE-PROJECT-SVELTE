<script>
  import { onMount } from 'svelte';
  import { saveAs } from 'file-saver';
import Papa from 'papaparse';

  let reservations = [];
  let timetables = [];
  let showTable = false; // Add showTable variable to control table visibility

  onMount(async () => {
    // Fetch reservations and timetables data
    const reservationsResponse = await fetch('http://localhost:4000/users/reservations');
    const timetablesResponse = await fetch('http://localhost:4000/users/timetables');

    reservations = await reservationsResponse.json();
    timetables = await timetablesResponse.json();
  });

  function getStatus(roomId, day, slot) {
    const reservation = reservations.find(reservation => reservation.room_id === roomId && reservation.reservation_date === day && reservation.slot === slot);
    const timetable = timetables.find(timetable => timetable.room_id === roomId && timetable.day === day && timetable.slot === slot);

    if (reservation || timetable) {
      
      return 'Occupied';
    } else {
      return 'Available';
    }
  }

  function generateOccupancyChart() {
    showTable = true; // Set showTable to true when button is clicked
  }
  async function exportTable() {
  const table = document.getElementById('tblExport1');
  const csv = Papa.unparse(tableToData(table));
  const blob = new Blob([csv], { type: 'text/csv;charset=utf-8' });
  saveAs(blob, 'occupancy_chart.csv');
}

function tableToData(table) {
  const rows = Array.from(table.getElementsByTagName('tr'));
  const data = [];

  for (let i = 0; i < rows.length; i++) {
    const row = rows[i];
    const rowData = [];

    const cells = row.getElementsByTagName('td');
    for (let j = 0; j < cells.length; j++) {
      rowData.push(cells[j].innerText);
    }

    data.push(rowData);
  }

  return data;
}
</script>

<style>
  .occupied {
    color: red;
  }

  .available {
    color: green;
  }
</style>
<br/><br/><br/><br/>
<main class="container">
  <button class="btn waves-effect waves-light light-blue" on:click="{generateOccupancyChart}">Generate Occupancy Chart</button>
  <!-- Add button to generate occupancy chart -->
  {#if showTable} <!-- Show table only if showTable is true -->
  <div class="card ">
    <div class="card-content">
      <table id="tblExport1" class="highlight">
        <thead>
          <tr>
            <th>Room ID</th>
            <th>Day</th>
            <th>Slot</th>
            <th>Status</th>
          </tr>
        </thead>
        <tbody>
          {#each timetables as timetable}
          <tr>
            <td>{timetable.room_id}</td>
            <td>{timetable.day}</td>
            <td>{timetable.slot}</td>
            <td class:occupied={getStatus(timetable.room_id, timetable.day, timetable.slot) === 'Occupied'} class:available={getStatus(timetable.room_id, timetable.day, timetable.slot) === 'Available'} >{getStatus(timetable.room_id, timetable.day, timetable.slot)}</td>
          </tr>
          {/each}
        </tbody>
      </table>
      
    </div>
    
  </div>
  <div>
    <button id="btnExport" class="btn waves-effect waves-light light-blue" on:click="{exportTable}">Export to csv</button>
  </div>
  {/if}
</main>
