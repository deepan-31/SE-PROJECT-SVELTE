<script>
  import { onMount } from 'svelte';

  let rooms = [];
  let filteredRooms = [];

  onMount(async () => {
    // Fetch list of rooms here and assign it to the `rooms` variable
    const response = await fetch('http://localhost:4000/users/roomd');
    const data = await response.json();
    rooms = data;
    filteredRooms = rooms; // Initialize filteredRooms with all rooms initially
  });

  function filterRooms() {
    const floorNoInput = document.getElementById('floorNo');
    const roomTypeInput = document.getElementById('roomType');
    const corridorIdInput = document.getElementById('corridorId');

    const floorNo = parseInt(floorNoInput.value);
    const roomType = roomTypeInput.value;
    const corridorId = corridorIdInput.value;

    filteredRooms = rooms.filter(room => {
      if (floorNoInput.value && room.floor_no !== floorNo) return false;
      if (roomTypeInput.value && room.room_type !== roomType) return false;
      if (corridorIdInput.value && room.corridor_id !== corridorId) return false;
      return true;
    });
  }
</script>

<br/><br/><br/>
<main class="container">
  <div class="row">
    <div class="input-field col s4">
      <input type="text" id="floorNo" on:input="{filterRooms}" />
      <label for="floorNo">Floor No</label>
    </div>
    <div class="input-field col s4">
      <input type="text" id="roomType" on:input="{filterRooms}" />
      <label for="roomType">Room Type</label>
    </div>
    <div class="input-field col s4">
      <input type="text" id="corridorId" on:input="{filterRooms}" />
      <label for="corridorId">Corridor ID</label>
    </div>
  </div>
  {#if filteredRooms.length > 0}
  <div class="col">
    <div class="row">
      {#each filteredRooms as room}
      <div class="col s12 m6 l4">

          <div class="card hoverable">
            <div class="card-image">
              <img src={room.image_url} alt={room.room_id} width="500" height="300" >
              <span class="card-title highlight teal"
        >{room.room_id}</span>
            </div>
            <div class="card-content teal lighten-3">
              <!--<h1>{room.room_id}</h1>-->
              <p>Floor No: {room.floor_no}</p>
              <p>Corridor ID: {room.corridor_id}</p>
              <p>Room Type: {room.room_type}</p>
              <p>Details: {room.details}</p>
            </div>
          </div>

      </div>
      {/each}
    </div>
  </div>
  {:else}
  <p>No rooms found.</p>
  {/if}
</main>
