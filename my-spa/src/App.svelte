<script>
	import { Router, Route, Link, link } from "svelte-routing";
	import Login from "./pages/Login.svelte";
	import About from "./pages/About.svelte";

	export let url = "";
</script>

<main>
	<Router {url}>
		<nav>
			<Link to="/">Login</Link>
			<Link to="about">About</Link>
		</nav>
		<div>
			<Route path="about" component={About} />
			<Route path="/"><Login /></Route>
			<Route path="/auth/callback" component={AuthCallback} />
		</div>
	</Router>
</main>

<style>
	main {
		text-align: center;
		padding: 1em;
		max-width: 240px;
		margin: 0 auto;
	}

	:global(h1) {
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
