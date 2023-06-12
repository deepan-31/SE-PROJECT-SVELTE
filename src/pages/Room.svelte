<script>
  export let room;
  export let handleRoomClick;
</script>

<div class="col s12 m6 l4">
  <div
    class="card large hoverable"
    on:click={() => handleRoomClick(room.room_id)}
    on:keydown={() => {}}
  >
    <div class="card-image">
      <img src={room.image_url} alt={room.room_id} width="500" height="300" />
      <span class="card-title highlight light-blue">{room.room_id}</span>
    </div>
    <div class="card-content"><p>{room.details}</p></div>
    {#if room.room_type === "classroom"}
      <div class="card-action"><p>This is a classroom</p></div>
    {/if}
  </div>
</div>

