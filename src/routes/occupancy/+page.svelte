<script>
  import { authHandlers } from "../../store/store";
  import Navbar from "../../components/Navbar.svelte";
  import Occupation from "../../pages/Occupation.svelte";
</script>

<title>OCCUPO | Occupancy</title>

<div style="position: fixed; top: 0; width: 100%; z-index: 10;">
  <Navbar {authHandlers} />
</div>
<div />
<Occupation/>