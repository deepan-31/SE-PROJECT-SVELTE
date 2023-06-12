<script>
    import { onMount } from "svelte";
  
    let room = {
      room_id: "",
      floor_no: "",
      corridor_id: "",
      room_type: "",
      details: "",
      image_url: "",
    };
  
    function submitForm() {
      // Perform form validation
      if (
        room.room_id === "" ||
        room.floor_no === "" ||
        room.corridor_id === "" ||
        room.room_type === "" ||
        room.details === "" ||
        room.image_url === ""
      ) {
        alert("Please fill in all fields");
        return;
      }
  
      // Create JSON object from form data
      const jsonData = JSON.stringify(room);
  
      // Make the POST request
      fetch("http://localhost:4000/users/room", {
        method: "POST",
        headers: {
          "Content-Type": "application/json",
        },
        body: jsonData,
      })
        .then((response) => response.json())
        .then((data) => {
          // Handle the response data
          alert(data.message); // Show the message from the server to the user
        })
        .catch((error) => {
          console.error(error);
          alert("An error occurred while submitting the form");
        });
    }
  
    onMount(() => {
      M.AutoInit();
    });
  </script>
  
  <br /><br />
  <div class="container">
    <div class="card">
      <div class="card-content">
        <h4>Add Room</h4>
        <form on:submit|preventDefault={submitForm}>
          <div class="input-field">
            <input type="text" id="room_id" bind:value={room.room_id} />
            <label for="room_id">Room ID</label>
          </div>
          <div class="input-field">
            <input type="number" id="floor_no" bind:value={room.floor_no} />
            <label for="floor_no">Floor Number</label>
          </div>
          <div class="input-field">
            <input type="text" id="corridor_id" bind:value={room.corridor_id} />
            <label for="corridor_id">Corridor ID</label>
          </div>
          <div class="input-field">
            <input type="text" id="room_type" bind:value={room.room_type} />
            <label for="room_type">Room Type</label>
          </div>
          <div class="input-field">
            <input type="text" id="details" bind:value={room.details} />
            <label for="details">Room Details</label>
          </div>
          <div class="input-field">
            <input type="text" id="image_url" bind:value={room.image_url} />
            <label for="image_url">Image URL</label>
          </div>
          <button class="btn waves-effect waves-light light-blue" type="submit">
            Submit
          </button>
        </form>
      </div>
    </div>
  </div>
  