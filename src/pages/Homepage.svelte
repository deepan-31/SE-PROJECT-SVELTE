<title>OCCUPO | Homepage</title>
<br />
<div class="banner-container">
  <img
    src="https://raw.githubusercontent.com/deepan-31/amrita_ab3images/main/AB3_courtyard.jpg"
    alt="Unsplashed background img 1"
    class="banner-image"
  />
  <div class="banner-overlay" />
  <div class="text-container">
    <h1 class="banner-title light-blue-text text-lighten-3">Welcome To Amrita Occupo</h1>
    <h5 class="banner-subtitle">
      We aim to streamline the room reservation process, enhance user
      experience, and provide a comprehensive timetable management solution
    </h5>
  </div>
</div>

<div class="container">
  <div class="section">
    <div class="row">
      <div class="col s12">
        <h4 class="header">Key Features:</h4>
        <table class="light-blue darken-1 responsive-table">
          <thead>
            <tr>
              <th>User Account Management</th>
              <th>Timetable Management</th>
              <th>Room Reservation</th>
              <th>Room Management</th>
              <th>Occupancy Chart Generation</th>
            </tr>
          </thead>
          <tbody>
            <tr>
              <td>User account creation and login</td>
              <td>Importing and validation of timetable data</td>
              <td>Real-time room availability check and reservation creation</td
              >
              <td>CRUD operations for managing rooms</td>
              <td>Generation of occupancy chart based on data</td>
            </tr>
          </tbody>
        </table>
        <table class="highlight">
          <thead>
            <tr>
              <th>Tools and Technologies</th>
              <th>Framework</th>
            </tr>
          </thead>
          <tbody>
            <tr>
              <td>Backend</td>
              <td
                >Node.js<img
                  class="logo"
                  src="https://nodejs.dev/static/images/brand/hexagon/green.svg"
                  alt="node"
                />
                Express.js,<img
                  class="logo"
                  src="https://img.icons8.com/?size=512&id=10263&format=png"
                  alt="express"
                />
                Firebase<img
                  class="logo"
                  src="https://firebase.google.com/static/downloads/brand-guidelines/SVG/logo-logomark.svg"
                  alt="firebase"
                />
              </td>
            </tr>
            <tr>
              <td>Databases</td>
              <td
                >MySQL<img
                  class="logo"
                  src="https://www.mysql.com/common/logos/powered-by-mysql-167x86.png"
                  alt="mysql"
                /></td
              >
            </tr>
            <tr>
              <td>Frontend</td>
              <td
                >Svelte<img
                  class="logo"
                  src="https://raw.githubusercontent.com/sveltejs/branding/ab67af031f2cf3c680431506a4e1ba24f7968d4d/svelte-logo.svg"
                  alt="Svelte Logo"
                /></td
              >
            </tr>
            <tr>
              <td>Testing Framework</td>
              <td
                >Mocha (for JavaScript)<img
                  class="logo"
                  src="https://raw.githubusercontent.com/mochajs/mocha/37deed262d4bc0788d32c66636495d10038ad398/assets/mocha-logo.svg"
                  alt="mocha"
                /></td
              >
            </tr>
          </tbody>
        </table>

        <p class="text">
          By implementing this project, we aim to streamline the room
          reservation process, enhance user experience, and provide a
          comprehensive timetable management solution. We are confident that our
          system will meet the needs of our users and contribute to efficient
          room utilization and scheduling within the organization.
        </p>
      </div>
    </div>
  </div>
</div>

<style>
  .logo {
    display: inline-block;
    width: 50px; /* Adjust the width as needed */
    height: 40px; /* Adjust the height as needed */
    margin-right: 5px; /* Adjust the spacing between image and text as needed */
  }

  .banner-container {
    position: relative;
    text-align: center;
  }

  .banner-image {
    width: 100%;
    height: 500px;
    object-fit: cover;
  }

  .text-container {
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    color: white;
    text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.6);
  }

  .banner-title {
    font-size: 60px;
    font-weight: bold;
    margin-bottom: 20px;
  }

  .banner-subtitle {
    font-size: 24px;
    margin-bottom: 40px;
  }

  .banner-overlay {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background-color: rgba(0, 0, 0, 0.4);
  }
</style>
