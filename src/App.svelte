<script>
    import Animation from './libs/animation.svelte';

    let scoops = 0;
    let yes = true
    let urlGenerator = "https://fabienbounoir.github.io/Bouns-Interaction-Twitch-Tchat/"
    let chaine = []
    let value = ""
    let config = {message:false,subscription:false,deleted:false,subgift:false,cheer:false,ban:false,timeout:false,dark:false,avatar:false,left:0,animSub:false,animSubGift:false,animCheer:false}

    function add()
    {
        if(value.length == 0) return

        chaine = [...chaine, value]
        value = ""
    }

    function remove(value)
    {
        chaine = chaine.filter(items => items != value)
    }

    function copy()
    {
        var text = document.getElementById("lien").innerHTML.replace("amp;","");

        navigator.clipboard.writeText(replaceStr(text, ["amp;"], ['',' '])).then(function() {
            console.log("Copie reussi")
        }, function() {
            console.log("Copie echoué")
        });
    }

    function replaceStr(str, find, replace) {
        for (var i = 0; i < find.length; i++) {
            str = str.replace(new RegExp(find[i], 'gi'), replace[i]);
        }
        return str;
    }

</script>

<Animation />
<main>
    <div>
        <h1>Configuration</h1>

        <p>Chaine à écouter:</p>
        <input class="check" type="text" placeholder="badbounstv" bind:value={value}/>
        <button on:click={add}>add</button>

        <div class="list">
            {#each chaine as value}
                <p on:click={() => {remove(value)}}>{value}</p>
            {/each}
        </div>

        <h2>Events:</h2>
        <div class="check">
            <div>
                <label>
                    <input type=checkbox bind:checked={config.message}>
                    Messages
                </label>
            </div>

            <div>
                <label>
                    <input type=checkbox bind:checked={config.subscription}>
                    subscription
                </label>
            </div>

            <div>
                <label>
                    <input type=checkbox bind:checked={config.subgift}>
                    subgift
                </label>
            </div>

            <div>
                <label>
                    <input type=checkbox bind:checked={config.cheer}>
                    cheer
                </label>
            </div>

            <div>
                <label>
                    <input type=checkbox bind:checked={config.ban}>
                    ban
                </label>
            </div>

            <div>
                <label>
                    <input type=checkbox bind:checked={config.timeout}>
                    Time out
                </label>
            </div>

            <div>
                <label>
                    <input type=checkbox bind:checked={config.deleted}>
                    deleted
                </label>
            </div>

        </div>

        <h2>Design:</h2>
        <div class="check">
            <div id="radio">
                <label>
                    <input type=radio bind:group={config.left} name="scoops" value={1}>
                    Left
                </label>
                
                <label>
                    <input type=radio bind:group={config.left} name="scoops" value={0}>
                    Right
                </label>
            </div>

            <div>
                <label>
                    <input type=checkbox bind:checked={config.dark}>
                    Dark mode
                </label>
            </div>

            <div>
                <label>
                    <input type=checkbox bind:checked={config.avatar}>
                    Avatar
                </label>
            </div>

            {#if config.subscription}
                <div>
                    <label>
                        <input type=checkbox bind:checked={config.animSub}>
                        Animation Sub
                    </label>
                </div>
            {/if}

            {#if config.subgift}
                <div>
                    <label>
                        <input type=checkbox bind:checked={config.animSubGift}>
                        Animation Sub Gift
                    </label>
                </div>
            {/if}

            {#if config.cheer}
                <div>
                    <label>
                        <input type=checkbox bind:checked={config.animCheer}>
                        Animation Cheer
                    </label>
                </div>
            {/if}
        </div>
    </div>

    <div>
        {#if chaine.length > 0}
        <h2>Lien:</h2>
            <p id='lien' on:click={copy}>{urlGenerator}?chaine={chaine.join()}{config.message ? ("&message=true") : ("")}{config.subscription ? ("&subscription=true") : ("")}{config.deleted ? ("&deleted=true") : ("")}{config.subgift ? ("&subgift=true") : ("")}{config.cheer ? ("&cheer=true") : ("")}{config.ban ? ("&ban=true") : ("")}{config.timeout ? ("&timeout=true") : ("")}{config.dark ? ("&dark=true") : ("")}{config.avatar ? ("&avatar=true") : ("")}{config.left ? ("&left=true") : ("")}{config.animSub ? ("&animsub=true") : ("")}{config.animSubGift ? ("&animsubgift=true") : ("")}{config.animCheer ? ("&animcheer=true") : ("")}</p>
            <button on:click={copy}>copy link</button>
        {/if}
    </div>
</main>

<style>
    main{
        display: grid;
        grid-template-columns: 0.5fr 1fr;
    }

    #radio label{
        margin-right: 1.5em;
    }
    :root {
        font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen,
        Ubuntu, Cantarell, 'Open Sans', 'Helvetica Neue', sans-serif;
    }

    #lien {
        margin: 0;
        width: 1fr;
        max-width: 40em;
    }

    div{
        /* border-bottom: 1px solid black; */
        color: #999CA0;
    }

    .check{
        margin-left: 10px;
    }

    input {
        color: #999CA0;
        padding: 4px 6px 4px 6px;
        background: #2C2828;
        border: 0px;
        box-shadow: 0px 48px 48px -6px rgba(0, 0, 0, 0.88), 0px 0px 1px rgba(0, 0, 0, 0.72);
        border-radius: 12px;
        margin: 10px 10px 10px 0px;
    }

    button{
        color: #999CA0;
        padding: 4px 6px 4px 6px;
        background: #2C2828;
        border: 0px;
        box-shadow: 0px 48px 48px -6px rgba(0, 0, 0, 0.88), 0px 0px 1px rgba(0, 0, 0, 0.72);
        border-radius: 12px;
        margin: 10px 10px 10px 0px;
    }

    button:hover{
        background: #595454;
    }

    h1, h2, p {
        color: #999CA0;
    }

    .list{
        display: flex;
        border-bottom: 0px solid black;
        /* display: grid;
        grid-template-columns: repeat(5, auto) */
    }

    .list p{
        padding: 4px 6px 4px 6px;
        background: #2C2828;

        box-shadow: 0px 48px 48px -6px rgba(0, 0, 0, 0.88), 0px 0px 1px rgba(0, 0, 0, 0.72);
        border-radius: 12px;
        margin: 10px 10px 10px 0px;
    }

    .list p:hover{
        background: #7b0000;
    }

    :global(body) {
		background-color: #1F1F1F;
	}

    @media screen and (max-width: 850px) {
        main{
            display: grid;
            grid-template-columns: auto;
        }
    }

</style>