<script>
  export let room;
  export let handleRoomClick;
</script>

<div class="col s12 m6 l4">
  <div class="card large">
    <div class="card-image">
      <img src={room.image_url} alt={room.room_id} width="500" height="300" />
      <span class="card-title"
        ><div class="card-panel teal lighten-2">{room.room_id}</div></span
      >
    </div>
    <div class="card-content">
      <p>{room.details}</p>
    </div>
    {#if room.room_type === "classroom"}
      <div class="card-action">
        <div
          class="waves-effect waves-light btn"
          on:click={() => handleRoomClick(room.room_id)}
          on:keydown={() => {}}
        >
        <i class="material-icons right">cloud</i>
          Timetable
        </div>

        <a href="/reservation">
          <div class="waves-effect waves-light btn" >
            <i class="material-icons right">send</i>
            Reservation
          </div>
        </a>
      </div>
    {/if}
  </div>
</div>
