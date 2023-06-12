<script>
  import { onMount } from "svelte";
  import Room from "./Room.svelte";
  import Timetable from "./Timetable.svelte";
  import Interactivetime from "../components/Interactivetime.svelte";

  let roomData = [];
  let selectedRoomId = null;
  let timetable = [];
  let showTimetable = false;
  let showInteractiveTime = false;

  async function fetchRoomData() {
    try {
      const response = await fetch("http://localhost:4000/users/rooms");
      const roomIds = await response.json();

      // Remove duplicate room IDs
      const uniqueRoomIds = [...new Set(roomIds)];

      // Fetch room details for each unique room ID
      const roomDetailsPromises = uniqueRoomIds.map(async (roomId) => {
        const roomResponse = await fetch(
          `http://localhost:4000/users/rooms/${roomId}`
        );
        const roomDetail = await roomResponse.json();
        return roomDetail;
      });

      // Wait for all room details to be fetched
      roomData = await Promise.all(roomDetailsPromises);
    } catch (err) {
      console.error(err);
    }
  }

  async function fetchTimetable(roomId) {
    try {
      const response = await fetch(
        `http://localhost:4000/users/timetable/${roomId}`
      );
      timetable = await response.json();
    } catch (err) {
      console.error(err);
    }
  }

  async function handleRoomClick(roomId) {
    selectedRoomId = roomId;
    await fetchTimetable(roomId);
    showTimetable = true;
  }

  function handleCancel() {
    showTimetable = false;
  }

  function toggleInteractiveTime() {
    showInteractiveTime = !showInteractiveTime;
  }

  onMount(async () => {
    await fetchRoomData();
  });
</script>

<br /><br /><br />
<div>
  {#if !showTimetable}
    <div class="row">
      {#each roomData as room}
        <Room {room} {handleRoomClick} />
      {/each}
    </div>
  {:else}
    {#if showInteractiveTime}
      <Interactivetime on:cancel={toggleInteractiveTime} />
    {:else}
      <button on:click={toggleInteractiveTime} class="btn purple">Show Interactive Time</button>
      <Timetable {timetable} on:cancel={handleCancel} />
    {/if}
    
  {/if}
</div>
