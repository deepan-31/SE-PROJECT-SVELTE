<script>
  import { authHandlers } from "../../store/store";
  import Navbar from "../../components/Navbar.svelte";
  import Timetableroom from "../../pages/Timetableroom.svelte";
</script>

<title>OCCUPO | Timetable</title>

<div style="position: fixed; top: 0; width: 100%; z-index: 10;">
  <Navbar {authHandlers} />
</div>
<div class="container">
  <Timetableroom />
</div>
