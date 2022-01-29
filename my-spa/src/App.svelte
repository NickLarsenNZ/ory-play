<script>
	import { Router, Route, Link } from "svelte-routing";
	import Home from "./pages/Home.svelte";
	import About from "./pages/About.svelte";

	export let name;
	export let url = "";
</script>

<main>
	<Router {url}>
		<nav>
			<Link to="/">Home</Link>
			<Link to="about">About</Link>
		</nav>
		<div>
			<Route path="about" component={About} />
			<Route path="/"><Home {name} /></Route>
		</div>
	</Router>
</main>

<style>
	main {
		text-align: left;
		padding: 1em;
		max-width: 240px;
		margin: 0 auto;
	}

	h1 {
		color: #ff3e00;
		text-transform: uppercase;
		font-size: 4em;
		font-weight: 100;
	}

	@media (min-width: 640px) {
		main {
			max-width: none;
		}
	}
</style>
