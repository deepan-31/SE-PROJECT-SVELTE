<script>
  import { onMount } from "svelte";
  import Addrooom from "../components/Addrooom.svelte";

  let rooms = [];
  let filteredRooms = [];
  let showAddRoom = false;
  let displayAsTable = false;

  onMount(async () => {
    // Fetch list of rooms here and assign it to the `rooms` variable
    const response = await fetch("http://localhost:4000/users/roomd");
    const data = await response.json();
    rooms = data;
    filteredRooms = rooms; // Initialize filteredRooms with all rooms initially
  });

  function filterRooms() {
    const floorNoInput = document.getElementById("floorNo");
    const roomTypeInput = document.getElementById("roomType");
    const corridorIdInput = document.getElementById("corridorId");

    const floorNo = parseInt(floorNoInput.value);
    const roomType = roomTypeInput.value;
    const corridorId = corridorIdInput.value;

    filteredRooms = rooms.filter((room) => {
      if (floorNoInput.value && room.floor_no !== floorNo) return false;
      if (roomTypeInput.value && room.room_type !== roomType) return false;
      if (corridorIdInput.value && room.corridor_id !== corridorId)
        return false;
      return true;
    });
  }

  function toggleAddRoom() {
    showAddRoom = !showAddRoom;
  }

  function toggleDisplayAsTable() {
    displayAsTable = !displayAsTable;
  }

  async function deleteRoom(roomId) {
    try {
      const response = await fetch(
        `http://localhost:4000/users/room/${roomId}`,
        {
          method: "DELETE",
        }
      );
      if (response.ok) {
        // Remove the deleted room from the filteredRooms array
        filteredRooms = filteredRooms.filter((room) => room.room_id !== roomId);
      } else {
        console.error(`Failed to delete room with ID: ${roomId}`);
      }
    } catch (err) {
      console.error(err);
    }
  }
</script>

<br /><br /><br />
<main class="container">
  <div class="row">
    <div class="col s12">
      {#if showAddRoom}
        <button class="btn red" on:click={toggleAddRoom}>Cancel</button>
      {:else}
        <div class="col s4">
          <button class="btn" on:click={toggleAddRoom}>Add Room</button>
        </div>
        <div class="col s4">
          <button class="btn" on:click={toggleDisplayAsTable}>
            {#if displayAsTable}
              Display as Cards
            {:else}
              Display as Table
            {/if}
          </button>
        </div>
      {/if}
    </div>
  </div>

  {#if showAddRoom}
    <Addrooom />
  {:else}
    <div class="row">
      <div class="input-field col s4">
        <input type="text" id="floorNo" on:input={filterRooms} />
        <label for="floorNo">Floor No</label>
      </div>
      <div class="input-field col s4">
        <input type="text" id="roomType" on:input={filterRooms} />
        <label for="roomType">Room Type</label>
      </div>
      <div class="input-field col s4">
        <input type="text" id="corridorId" on:input={filterRooms} />
        <label for="corridorId">Corridor ID</label>
      </div>
    </div>
    {#if filteredRooms.length > 0}
      {#if displayAsTable}
        <div class="card z-depth-3">
          <div class="card-content">
            <h5>Rooms table</h5>
            <table class="striped">
              <thead>
                <tr>
                  <th>Room ID</th>
                  <th>Floor No</th>
                  <th>Corridor ID</th>
                  <th>Room Type</th>
                  <th>Details</th>
                  <th>Actions</th>
                </tr>
              </thead>
              <tbody>
                {#each filteredRooms as room}
                  <tr>
                    <td>{room.room_id}</td>
                    <td>{room.floor_no}</td>
                    <td>{room.corridor_id}</td>
                    <td>{room.room_type}</td>
                    <td>{room.details}</td>
                    <td>
                      <button
                        class="btn red"
                        on:click={() => deleteRoom(room.room_id)}>Delete</button
                      >
                    </td>
                  </tr>
                {/each}
              </tbody>
            </table>
          </div>
        </div>
      {:else}
        <div class="col">
          <div class="row">
            {#each filteredRooms as room}
              <div class="col s12 m6 l4">
                <div class="card large light-blue lighten-3">
                  <div class="card-image">
                    <img
                      src={room.image_url}
                      alt={room.room_id}
                      width="500"
                      height="300"
                    />
                    <span class="card-title highlight light-blue"
                      >{room.room_id}</span
                    >
                  </div>
                  <div class="card-content">
                    <p>Floor No: {room.floor_no}</p>
                    <p>Corridor ID: {room.corridor_id}</p>
                    <p>Room Type: {room.room_type}</p>
                    <p>Details: {room.details}</p>
                  </div>
                  <div class="card-action">
                    <button
                      class="btn red"
                      on:click={() => deleteRoom(room.room_id)}>Delete</button
                    >
                  </div>
                </div>
              </div>
            {/each}
          </div>
        </div>
      {/if}
    {:else}
      <p>No rooms found.</p>
    {/if}
  {/if}
</main>
