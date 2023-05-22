<script>
  export let room;
  export let handleRoomClick;
</script>

<div class="col s12 m6 l4">
  <div class="card large hoverable">
    <div class="card-image ">
      <img src={room.image_url} alt={room.room_id} width="500" height="300" />
      <span class="card-title highlight teal"
        >{room.room_id}</span
      >
    </div>
    <div class="card-content">
      <p>{room.details}</p>
    </div>
    {#if room.room_type === "classroom"}
      <div class="card-action">
        <div class="btn-container">
          <div
            class="waves-effect waves-light btn"
            on:click={() => handleRoomClick(room.room_id)}
            on:keydown={() => {}}
          >
            <!--<i class="material-icons right">cloud</i>-->
            Timetable
          </div>

          <a href="/reservation">
            <div class="waves-effect waves-light btn">
              <!--<i class="material-icons right">send</i>-->
              Reservation
            </div>
          </a>
        </div>
      </div>
    {/if}
  </div>
</div>

<style>
  .btn-container {
    display: flex;
    justify-content: space-between;
  }
</style>
