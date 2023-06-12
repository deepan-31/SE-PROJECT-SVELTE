<script>
    let roomOptions = [];
    let selectedRoom = "";
    let timetableData = [];
    import { createEventDispatcher } from "svelte";

const dispatch = createEventDispatcher();

function handleGoBack() {
  dispatch("goback");
}
    const fetchRoomOptions = async () => {
      try {
        const response = await fetch("http://localhost:4000/users/roomd");
        const allRoomOptions = await response.json();

        // Filter room options based on room_type
        roomOptions = allRoomOptions.filter((room) => room.room_type === "classroom");
      } catch (error) {
        console.error(error);
      }
    };
  
    const fetchTimetable = async () => {
      if (selectedRoom) {
        try {
          const response = await fetch(
            `http://localhost:4000/users/timetables/${selectedRoom}`
          );
          timetableData = await response.json();
        } catch (error) {
          console.error(error);
        }
      } else {
        timetableData = [];
      }
    };
  
    const handleRoomSelection = () => {
      fetchTimetable();
    };
  
    const handleInsert = async (slot, day) => {
      const course_code = prompt("Enter course code:");
      const course_name = prompt("Enter course name:");
      const room_id = selectedRoom;
  
      // Check if the inputs are not empty
      if (course_code && course_name && room_id) {
        const insertData = { course_code, course_name, room_id, day, slot };
  
        try {
          const response = await fetch("http://localhost:4000/users/timetables", {
            method: "POST",
            headers: { "Content-Type": "application/json" },
            body: JSON.stringify(insertData),
          });
  
          if (response.ok) {
            M.toast({
              html: "Timetable entry inserted",
              classes: "green darken-1",
            });
            fetchTimetable();
          } else {
            console.error("Failed to insert timetable entry");
          }
        } catch (error) {
          console.error(error);
        }
      }
    };
  
    const handleUpdate = async (timetableId) => {
      const existingEntry = timetableData.find((t) => t.timetable_id === timetableId);
      const course_code = prompt("Enter updated course code:", existingEntry.course_code);
      const course_name = prompt("Enter updated course name:", existingEntry.course_name);
      const room_id = selectedRoom;
      const day = existingEntry.day;
      const slot = existingEntry.slot;
  
      // Check if the inputs are not empty
      if (course_code && course_name && room_id) {
        const updateData = { course_code, course_name, room_id, day, slot };
  
        try {
          const response = await fetch(
            `http://localhost:4000/users/timetables/${timetableId}`,
            {
              method: "PUT",
              headers: { "Content-Type": "application/json" },
              body: JSON.stringify(updateData),
            }
          );
  
          if (response.ok) {
            M.toast({
              html: "Timetable entry updated",
              classes: "blue darken-1",
            });
            fetchTimetable();
          } else {
            console.error("Failed to update timetable entry");
          }
        } catch (error) {
          console.error(error);
        }
      }
    };
  
    const handleDelete = async (timetableId) => {
      const confirmation = confirm("Are you sure you want to delete this timetable entry?");
  
      if (confirmation) {
        try {
          const response = await fetch(`http://localhost:4000/users/timetables/${timetableId}`, {
            method: "DELETE",
          });
  
          if (response.ok) {
            M.toast({
              html: "Timetable entry deleted",
              classes: "red darken-1",
            });
            fetchTimetable();
          } else {
            console.error("Failed to delete timetable entry");
          }
        } catch (error) {
          console.error(error);
        }
      }
    };
  
    fetchRoomOptions();
  </script>
  
  <main>
    <button on:click={handleGoBack} class="btn red">Cancel</button>
    <div class="card z-depth-3">
      <div class="card-content">
        <h1>Interactive Timetable</h1>
  
        <div class="input-field">
          <select class="browser-default" bind:value={selectedRoom} on:change={handleRoomSelection}>
            <option value="">Select a room</option>
            {#each roomOptions as room}
            <option value={room.room_id}>{room.room_id}</option>
            {/each}
          </select>
        </div>
  
        <table class="striped">
          <thead>
            <tr>
              <th>Slot</th>
              <th>Monday</th>
              <th>Tuesday</th>
              <th>Wednesday</th>
              <th>Thursday</th>
              <th>Friday</th>
              <th>Saturday</th>
            </tr>
          </thead>
          <tbody>
            {#each Array(8) as _, slot}
            <tr>
              <td>{slot + 1}</td>
              {#each ["monday", "tuesday", "wednesday", "thursday", "friday", "saturday"] as day}
              {#if timetableData.find((t) => t.day === day && t.slot === slot)}
              <td>
                <div>{timetableData.find((t) => t.day === day && t.slot === slot).course_code}</div>
                <div>{timetableData.find((t) => t.day === day && t.slot === slot).course_name}</div>
                <!--<div>{timetableData.find((t) => t.day === day && t.slot === slot).room_id}</div>
                <div>{timetableData.find((t) => t.day === day && t.slot === slot).day}</div>
                <div>{timetableData.find((t) => t.day === day && t.slot === slot).slot}</div>-->
                <div>
                  <button class="waves-effect waves-light btn-small red" on:click={() => handleDelete(timetableData.find((t) => t.day === day && t.slot === slot).timetable_id)}>
                    <i class="material-icons">delete</i>
                  </button>
                  <button class="waves-effect waves-light btn-small blue" on:click={() => handleUpdate(timetableData.find((t) => t.day === day && t.slot === slot).timetable_id)}>
                    <i class="material-icons">edit</i>
                  </button>
                </div>
              </td>
              {:else}
              <td>
                <button class="waves-effect waves-light btn-small" on:click={() => handleInsert(slot, day)}><i class="material-icons">add</i></button>
              </td>
              {/if}
              {/each}
            </tr>
            {/each}
          </tbody>
        </table>
      </div>
    </div>
  </main>
  
  <style>
    main {
      padding: 2rem;
    }
  </style>
  