<script>
  import { onMount } from "svelte";

  let reservations = [];
  let reservationStatuses = {};

  let reservation = {
    user_id: "",
    slot: "",
    reservation_date: "",
    room_id: "",
    reason_for_reservation: "",
  };
  function submitForm() {
  // Perform form validation
  if (
    reservation.user_id === "" ||
    reservation.slot === "" ||
    reservation.reservation_date === "" ||
    reservation.room_id === "" ||
    reservation.reason_for_reservation === ""
  ) {
    alert("Please fill in all fields");
    return;
  }

  // Create JSON object from form data
  const jsonData = JSON.stringify(reservation);

  // Make the POST request to create a new reservation
  fetch("http://localhost:4000/users/reservations", {
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
      reservation = {
        user_id: "",
        slot: "",
        reservation_date: "",
        room_id: "",
        reason_for_reservation: "",
      }; // Reset the form fields
      fetchReservationStatus(); // Refresh the reservation status after creating a new reservation
      showReservations(); // Refresh the reservations table

      // Fetch the reservation ID for the new entry
      fetch("http://localhost:4000/users/reservations/latest")
        .then((response) => response.json())
        .then((latestReservation) => {
          const reservationId = latestReservation.reservation_id;
          const status = "Pending"; // Set the initial status as "Pending"

          insertReservationStatus(reservationId, status); // Insert the reservation status
        })
        .catch((error) => {
          console.error(error);
          alert("An error occurred while fetching the latest reservation");
        });
    })
    .catch((error) => {
      console.error(error);
      alert("An error occurred while submitting the form");
    });
}


  function showReservations() {
    // Make the GET request to fetch reservations
    fetch("http://localhost:4000/users/reservations")
      .then((response) => response.json())
      .then((data) => {
        // Update the reservations array
        reservations = data;
      })
      .catch((error) => {
        console.error(error);
        alert("An error occurred while fetching reservations");
      });
  }
  function insertReservationStatus(reservationId, status) {
  const statusData = JSON.stringify({ reservation_id: reservationId, status });

  // Make the POST request to insert the new reservation status
  fetch("http://localhost:4000/users/reservation-status", {
    method: "POST",
    headers: {
      "Content-Type": "application/json",
    },
    body: statusData,
  })
    .then((response) => response.json())
    .then((data) => {
      // Handle the response data
      alert(data.message); // Show the message from the server to the user
      fetchReservationStatus(); // Refresh reservation status after inserting
    })
    .catch((error) => {
      console.error(error);
      alert("An error occurred while inserting reservation status");
    });
}


  function fetchReservationStatus() {
    // Make the GET request to fetch reservation status
    fetch("http://localhost:4000/users/reservation-status")
      .then((response) => response.json())
      .then((data) => {
        // Create a mapping of reservation IDs and their corresponding status
        reservationStatuses = data.reduce((map, status) => {
          map[status.reservation_id] = status.status;
          return map;
        }, {});
      })
      .catch((error) => {
        console.error(error);
        alert("An error occurred while fetching reservation status");
      });
  }

  function updateReservationStatus(reservationId, status) {
    // Make the PUT request to update reservation status
    const jsonData = JSON.stringify({ status });

    fetch(`http://localhost:4000/users/reservation-status/${reservationId}`, {
      method: "PUT",
      headers: {
        "Content-Type": "application/json",
      },
      body: jsonData,
    })
      .then((response) => response.json())
      .then((data) => {
        alert(data.message);
        fetchReservationStatus(); // Refresh reservation status after updating
      })
      .catch((error) => {
        console.error(error);
        alert("An error occurred while updating reservation status");
      });
  }

  onMount(() => {
    M.AutoInit();
    fetchReservationStatus();
  });
</script>

<br /><br />
<div class="container">
  <div class="card">
    <div class="card-content">
      <h4>Reservation Form</h4>
      <form on:submit|preventDefault={submitForm}>
        <div class="input-field">
          <input type="text" id="user_id" bind:value={reservation.user_id} />
          <label for="user_id">User ID</label>
        </div>
        <div class="input-field">
          <select id="slot" bind:value={reservation.slot}>
            <option value="" disabled selected>Select a slot</option>
            <option value="1">Slot 1</option>
            <option value="2">Slot 2</option>
            <option value="3">Slot 3</option>
            <option value="4">Slot 4</option>
            <option value="5">Slot 5</option>
            <option value="6">Slot 6</option>
            <option value="7">Slot 7</option>
            <option value="8">Slot 8</option>
          </select>
          <label for="slot">Slot</label>
        </div>
        <div class="input-field">
          <input
            type="date"
            id="reservation_date"
            bind:value={reservation.reservation_date}
          />
          <label for="reservation_date">Reservation Date</label>
        </div>

        <div class="input-field">
          <input type="text" id="room_id" bind:value={reservation.room_id} />
          <label for="room_id">Room ID</label>
        </div>
        <div class="input-field">
          <input
            type="text"
            id="reason_for_reservation"
            bind:value={reservation.reason_for_reservation}
          />
          <label for="reason_for_reservation">Reason for Reservation</label>
        </div>
        <button class="btn waves-effect waves-light light-blue" type="submit">Submit</button>
      </form>
    </div>
  </div>
</div>

<!-- Show Reservations button -->
<div class="container">
  <button class="btn btn-large waves-effect waves-light light-blue" on:click={showReservations}>
    {#if reservations.length > 0}
      Hide Reservations
    {:else}
      Show Reservations
    {/if}
  </button>
</div>

<!-- Reservations Table -->
{#if reservations.length > 0}
  <div class="container">
    <div class="card">
      <div class="card-content">
        <h5>Reservations</h5>
        <table class="highlight">
          <thead>
            <tr>
              <th>Reservation ID</th>
              <th>User ID</th>
              <th>Slot</th>
              <th>Reservation Date</th>
              <th>Room ID</th>
              <th>Reason for Reservation</th>
              <th>Status</th>
              <th>Actions</th>
            </tr>
          </thead>
          <tbody>
            {#each reservations as reservation}
              <tr>
                <td>{reservation.reservation_id}</td>
                <td>{reservation.user_id}</td>
                <td>{reservation.slot}</td>
                <td>{reservation.reservation_date}</td>
                <td>{reservation.room_id}</td>
                <td>{reservation.reason_for_reservation}</td>
                <td>{reservationStatuses[reservation.reservation_id] || 'Pending'}</td>
                <td>
                  <select class="browser-default"
                    value={reservationStatuses[reservation.reservation_id] || 'Pending'}
                    on:change={(event) =>
                      updateReservationStatus(
                        reservation.reservation_id,
                        event.target.value
                      )
                    }
                  >
                    <option value="Pending">Pending</option>
                    <option value="Accepted">Accepted</option>
                    <option value="Rejected">Rejected</option>
                    <option value="Expired">Expired</option>
                  </select>
                </td>
              </tr>
            {/each}
          </tbody>
        </table>
      </div>
    </div>
  </div>
{/if}

<style>
  .card-content {
    padding: 20px;
  }
</style>
