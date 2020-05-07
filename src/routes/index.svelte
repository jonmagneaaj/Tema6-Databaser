
<svelte:head>
	<title>Sapper project template</title>
</svelte:head>

<script>
	import Gamebx from '../components/GameBoxes.svelte'
	import {onMount} from 'svelte'

	let games
	let db 

	let GameBoxes = []

	onMount(() => {
        db = firebase.firestore()
		games = db.collection('games')
		
		games.onSnapshot(snap => {
			GameBoxes = snap.docs
		})
	})
	

	const editGame = (evt) =>{
		edit = !edit

		name = evt.name
		description = evt.description
		//regelArray = evt.regelArray
		random = evt.random
		useName = evt.useName
	}



</script>
<main>
	<section>
		{#each GameBoxes as GameBox }
			<Gamebx id={GameBox.id} on:toggle={editGame} GameData={GameBox.data()} on:id={editGame} />
		{:else}
			<h2>Loading games...</h2>
		{/each}
	</section>
	
</main>


<style>
	main{
		padding-top: 2rem;
		margin: 0 auto;
		justify-content: center;
		width: 100rem;
		min-height: 100vh;
	}

	section{
		display: grid;
		grid-template-columns: repeat(auto-fill, minmax(20rem, 30rem));
		grid-template-rows: repeat(auto-fill, 40rem);
		gap: 1rem;
	}

	h2{
		color: white;
	}

</style>
