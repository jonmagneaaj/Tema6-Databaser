<script>
	export let segment;
</script>

<style>
	nav {
		background-color: var(--Purple);
		font-weight: 300;
		padding: 0 1em;

		height: 100vh;
		width: 8rem;
		z-index: 99;
		display: grid;
		grid-template-columns: repeat(auto-fill, minmax(8rem, 1fr));
		position: fixed;
	}

	ul {
		margin: 0;
		padding: 0;
	}

	/* clearfix */
	ul::after {
		content: '';
		display: block;
		clear: both;
	}

	li {
		display: block;
		float: left;
	}

	[aria-current] {
		position: relative;
		display: inline-block;
	}

	[aria-current]::after {
		position: absolute;
		content: '';
		width: calc(100% - 1em);
		height: 2px;
		background-color: var(--Pink);
		display: block;
		bottom: -1px;
	}

	a {
		text-decoration: none;
		padding: 1em 0.5em;
		display: block;
		color: white;
	}

	h2{
		color: var(--Pink);
		font-family: 'Indie Flower';
	}
</style>

<nav>
	<ul>
		<h2>Splash</h2>
		<li><a aria-current='{segment === undefined ? "page" : undefined}' href='.'>home</a></li>
		<li><a aria-current='{segment === "create" ? "page" : undefined}' href='create'>Add</a></li>
	</ul>
</nav>
