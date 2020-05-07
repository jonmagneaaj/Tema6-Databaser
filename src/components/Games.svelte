<svelte:head>
    <title>Your games</title>
</svelte:head>


<script>
    import {onMount} from 'svelte'

    let db 
    let games

    let name =''
    let description =''

    let regel = ''
    let regelArray = []

    let random = false
    let useName = false

    onMount(() => {
        db = firebase.firestore()
        games = db.collection('games')
    })

    const regPage = () =>{
        regelArray = [...regelArray, regel]
        regel = ''
    }

    const regGame = () =>{
        games
            .doc()
            .set({
                name: name,
                description : description,
                rules : regelArray,
                random : random,
                useName : useName
            })
            name = ''
            description = ''
            regelArray = []
            random = false
            useName = false

            alert('Game has been added to the database');
    }


</script>

<h1> Create your awesome game!</h1>

<div id='creator'>
    <div id='top'>
        <input bind:value={name} type='text' placeholder='Name of the game'>
        <input bind:value={description} type='text' placeholder='Description'>
    </div>
    <div id='display'>
        <div id='print'>
        <!--Prints out each rule-->
            {#each regelArray as regarr, index}
                <div class='regler'>
                    <div class='long'>{regarr}</div>
                    <button on:click={() => {regelArray.splice(index, 1); regelArray = regelArray;}}>X</button>
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
                <input type='checkbox' bind:checked={random}>
                Random order
            </label>
            <label>
                <input type='checkbox' bind:checked={useName}>
                Use names
            </label>
        </div>
    </div>
    <button on:click={regGame}>PUBLISH</button>
</div>

<style>

    label{
        color: white;
    }

    #print{
       
        display: grid;
        grid-template-columns: repeat(auto-fill, minmax(10rem, 1fr));
        grid-template-rows: repeat(auto-fill, 4rem);
        align-items: start;
        gap: 1rem;
        padding: 1rem;
        overflow: hidden;
        overflow-y: scroll;
        width: 100%;
    }

    .regler{
        text-align: center;
        background-color: var(--Purple);
        color: white;
        height: 3.8rem;
        font-size: 1rem;
        border-radius: 2rem;

        display: grid;
        grid-auto-flow: column;
        align-content: center;
    }

    #creator .regler button{
        color:white;
        background-color: var(--Purple);
        text-align: center;
        border: none;
        font-size: 1rem;
        font-weight: normal;

        height: 2rem;
        width: 2rem;

        right: -2rem;
        justify-self: end;
    }

    .long{
        text-overflow: ellipsis;
        white-space: nowrap;
        overflow: hidden;

        font-size: 1.2rem;
    }

    #creator{
        display: grid;
        justify-content: center;
        justify-items: center;
        
        grid-auto-flow: row;
        height: 50rem;
        width: 100%;

        background-color: var(--DarkPurple);

    }

    #creator button {
        border: none;
        height: 4.6rem;
        width: 10rem;
        background-color: var(--Pink);
        color: white;
        text-align: center;
        appearance: none;
        border-radius: 2rem;
    }

    #add form input{
        background-color: white;
        border-radius: 2rem;
        appearance: none;
        border: none;

        height: 4.4rem;
        width: 21.4rem;
        font-size: 1.6rem;
    }

    #add form button{
        border: none;
        height: 4.6rem;
        width: 5.1rem;
        background-color: var(--Pink);
        color: white;
        text-align: center;
        appearance: none;
        border-radius: 2rem;
    }
    input:focus{
        outline: none;
    }

    h1{
        text-align: center;
        color: var(--Pink);
    }
    ::placeholder{
        color: white;
    }

    #display{
        width: 100%;
        display: grid;
        justify-content: center;
        height: 50rem;

        border: 2px solid var(--Purple);
        border-radius: 20px;
        margin: 2rem;
    }

    #top{
        width: 100%;
        height: 3.9rem;
        display: grid;
        grid-template-columns: 4fr 6fr;
        gap: 2rem;
        margin: 2rem 0rem;
    }


    #top input{
        
        border: 0.2rem solid var(--Purple);
        border-radius: 2rem;

        color: white;
        background-color: var(--DarkPurple);
        font-size: 1.6rem;

        padding: 1rem;
    }

    #bottom{
        align-self: end;
        padding-top: 1rem;
    }

</style>