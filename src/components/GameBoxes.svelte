<script>
    import {onMount} from 'svelte'


    let games
    let db
    let update
    let regel=''

	onMount(() => {
        db = firebase.firestore()
        games = db.collection('games')

    update = () => {
        const game = games.doc(id)
        game.update({
            name: GameData.name,
            description: GameData.description,
            rules: GameData.rules,
            random: GameData.random,
            useName: GameData.useName
            })
        }
    })

   
    export let GameData = {}
    export let id = ''

    const deleteGame = () =>{
        const game = games.doc(id)
        game.delete()
    }

    let edit = false;

	const regPage = () =>{
        GameData.rules = [...GameData.rules, regel]
        regel = ''
    }
    
    const editGame = () =>{
        edit = !edit
    }

</script>


<section>
    {#if edit}
        <div id='top'>
				<input bind:value={GameData.name} type='text'>
				<input bind:value={GameData.description} type='text'>
			</div>
            <p>Rules:</p>
				<div id='print'>
				<!--Prints out each rule-->
					{#each GameData.rules as regarr, index}
						<div class='regler'>
							<input class='long' placeholder={regarr} bind:value={regarr}>
							<button on:click={() => {GameData.rules.splice(index, 1); GameData.rules = GameData.rules;}}>X</button>
						</div>
					{/each}
				</div>
				<!--Adds rule-->
				<div id='bottom'>
					<div id='add'>
						<form on:submit|preventDefault={regPage}>
							<input bind:value={regel} placeholder='Add rule'>
							<button>ADD</button>
						</form>
					</div>
					<label>
						<input type='checkbox' bind:checked={GameData.random}>
						Random order
					</label>
					<label>
						<input type='checkbox' bind:checked={GameData.useName}>
						Use names
					</label>
				</div>
            <div>
                <div id='cenEnd'>
                    <button on:click={() =>{update; edit = !edit}}>SAVE</button>
                </div>
                <div id='end'>
                    <button class='delete' on:click={editGame}>cancel</button>
                </div>
            </div>
    {:else}
        <div id='top'>
            <h2>{GameData.name}</h2>
            <p>{GameData.description}</p>
        </div>
        <div id='center'>
            <h2>{GameData.rules.length}</h2>
            <h3>RULES</h3>
        </div>
        <div>
            <div id='cenEnd'>
                <button on:click={editGame}> Edit </button>
            </div>
            <div id='end'>
                <button on:click={deleteGame} class='delete'>Delete</button>
            </div>
        </div>
	{/if}
</section>

<style>
    section{
        background-color: var(--Purple);
        border-radius: 2rem;
        display: grid;
        justify-content: center;
        gap: 1rem;

        text-align: center;

        padding: 1rem;
        
    }

    #top input{
        background-color: var(--Pink);
        border-radius: 2rem;
        color: white;
        border: none;
        font-size: 1.4rem;
    }
    #print{
        max-height: 8rem;
        overflow: hidden;
        overflow-y: scroll;

        display: grid;
        gap: 0.2rem;
    }

    .regler{
        background-color: var(--Pink);
        border-radius: 2rem;
        color: white;
        height: 2rem;
        text-align: center;

        display: grid;
        grid-auto-flow: column;
        align-content: center;
    }

    .regler input,button {
        background-color: var(--Pink);
        color: white;
        border: none;
        appearance: none;
    }


    h2{
        color: var(--Pink)
    }

    h3{
        color: white;
    }
    p{
        color: white;
    }

    label{
        color: white;
    }


   #cenEnd button {
        border: none;
        height: 2rem;
        width: 5rem;
        background-color: var(--Pink);
        color: white;
        text-align: center;
        appearance: none;
        border-radius: 2rem;
    }

    .regler{
        background-color: var(--Purple);
        border: none;
        appearance: none;
        border-radius: 2rem;
    }

    .delete{
        appearance: none;
        border: none;
        color: var(--Pink);
        background-color: var(--Purple);
        cursor: pointer;
    }
</style>

