<script>
  import { onMount } from 'svelte';

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

    if (reservation) {
      return 'Occupied';
    } else if (timetable) {
      return 'Available';
    } else {
      return 'N/A';
    }
  }

  function generateOccupancyChart() {
    showTable = true; // Set showTable to true when button is clicked
  }
</script>

<style>
  .occupied {
    color: red;
  }

  .available {
    color: green;
  }

  .na {
    color: gray;
  }
</style>
<br/><br/><br/><br/>
<main class="container">
  <button class="btn waves-effect waves-light" on:click="{generateOccupancyChart}">Generate Occupancy Chart</button>
  <!-- Add button to generate occupancy chart -->
  {#if showTable} <!-- Show table only if showTable is true -->
  <div class="card ">
    <div class="card-content">
      <table class="highlight">
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
            <td class:occupied={getStatus(timetable.room_id, timetable.day, timetable.slot) === 'Occupied'} class:available={getStatus(timetable.room_id, timetable.day, timetable.slot) === 'Available'} class:na={getStatus(timetable.room_id, timetable.day, timetable.slot) === 'N/A'}>{getStatus(timetable.room_id, timetable.day, timetable.slot)}</td>
          </tr>
          {/each}
        </tbody>
      </table>
    </div>
  </div>
  {/if}
</main>
