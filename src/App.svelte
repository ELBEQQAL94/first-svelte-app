<script>
import '../node_modules/materialize-css/dist/css/materialize.min.css';
import '../node_modules/materialize-css/dist/js/materialize.min.js';
import { Router, Link, Route } from "svelte-routing";
import Navbar from './components/Navbar.svelte';

// our pages
import Home from './pages/home/Home.svelte';
import About from './pages/about/About.svelte';

</script>

<style>
</style>

<Router>
    <Navbar />
	<div class="container">
	  <Route path="/" component={Home} />
	  <Route path="/about" component={About} />
	</div>
</Router>