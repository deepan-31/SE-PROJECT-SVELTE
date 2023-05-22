<script>
    let date = '';
    let rooms = [];
  
    async function fetchOccupation() {
      try {
        const response = await fetch(`http://localhost:4000/users/occupation?date=${date}`);
        const data = await response.json();
        rooms = data;
      } catch (error) {
        console.error('Error fetching occupation:', error);
      }
    }
  
    function handleDateChange(event) {
      date = event.target.value;
      fetchOccupation();
    }
  </script>
  
  <h1>Occupation</h1>
  
  <label for="date">Select a Date:</label>
  <input type="date" bind:value={date} id="date" on:change={handleDateChange}>
  
  {#if rooms.length === 0}
    <p>Loading occupation...</p>
  {:else}
    {#each rooms as room}
      <div>
        <h3>{room.room_name}</h3>
        <p>Available: {room.available ? 'Yes' : 'No'}</p>
      </div>
    {/each}
  {/if}
  