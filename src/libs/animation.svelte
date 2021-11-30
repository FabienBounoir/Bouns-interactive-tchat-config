<script>
    import { fade,scale,slide } from 'svelte/transition';
    import { v4 } from 'uuid';

    import fete from '../assets/animation/fete.png'
    import hop from '../assets/animation/hop.png'
    import love from '../assets/animation/love.png'
    import nice from '../assets/animation/nice.png'
    import yo from '../assets/animation/yo.png'
    import explosion from '../assets/animation/explosion.png'

    let actualPicture = yo
    let tchat = [];

    let speech = [
        {message:"Oh bienvenue à toi ❤️",avatar:love, time:12000},
        {message:"J'espere que tu vas bien",avatar:fete, time:12000},
        {message:"Tu es actuellement sur la page pour me configurer",avatar:nice, time:20000},
        {message:"Pour cela rien de plus simple il suffit de choisir les chaines sur lesquelles tu veux que j'écoute",avatar:yo, time:20000},
        {message:"Puis de choisir les interactions que tu veux",avatar:explosion, time:18000},
        {message:"Et pour finir de copier le lien est de l'intégrer dans une source navigateur sur OBS",avatar:hop, time:17000},
    ]

    let time = 1000

    const push = (snack, time=5000) => {      
        snack._id = v4();
        tchat = [...tchat, snack];

        setAvatar(snack.avatar)

		setTimeout(() => {
			tchat = tchat.filter((s) => s._id !== snack._id);

            if(tchat.length == 0)
            {
                speechWrite()
            }
		}, snack.time);
	};

    function setAvatar(avatar){
        actualPicture = avatar
    }

    function speechWrite()
    {
        time = 5000

        for(let i = 0; i < speech.length; i++)
        {
            setTimeout(() => {
                push(speech[i])
            }, time);

            time += 4000
        }
    }

    speechWrite()

</script>

<div id="rendu">
    <div class="gridApp">
        <div class="textfields" >
            <ul>
                {#each tchat as message (message._id)}
                    <li in:scale="{{ delay: tchat.length > 1 ? (0) : (380), duration: 500 }}" out:slide>
                        <p in:fade="{{ duration: 200 }}">{message.message} </p>
                    </li>
                {/each}
            </ul>
        </div>
        <div class="avatar">
            {#if tchat.length}
                <img in:scale="{{ duration: 400 }}" out:scale="{{ delay:500, duration: 1000 }}" src={actualPicture} alt=" "/>
            {/if}
        </div>
    </div>
</div>


<style>
    :root {
        font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen,
        Ubuntu, Cantarell, 'Open Sans', 'Helvetica Neue', sans-serif;
    }

    *,
    *::after,
    *::before {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
    }

    :global(body) {
        overflow-x: hidden;
	}

    div#rendu {
        position: fixed;
        top: 0;
        left: 0;
        width: 100%;
        height: 100%;
        /* background-image: url("../assets/background.png"); */
        background-size: cover;
        z-index: -1;
    }

    .gridApp{
        display: grid;
        /* grid-template-areas: "text avatar"; */
        grid-template-columns: 1fr 0.1fr;
        position: absolute;
        right: 0;
        bottom: 0;
    }

    .textfields {
        padding: 3em 3em 0em 3em;
        margin: -3em -4em 0em -3em;

        overflow: hidden; 
        overflow-y: auto; 
    }

    .avatar {
        place-self: flex-end;
    }

    .avatar img{
        border-radius: 100px;
        width: 10em;
    }

    ul{
        text-align: -webkit-right;
    }

    li{
        list-style-type: none;

        backdrop-filter: blur( 6px );
        -webkit-backdrop-filter: blur( 6px );
        margin: 10px;
        font-style: normal;
        font-weight: 300;
        font-size: 20px;
        line-height: 23px;

        width: max-content;
        max-width: 25em;

        text-align: right;

        transform-origin: bottom right;

        border-radius: 10px;

        color: #fff;
        border: 1px solid rgba( 255, 255, 255, 0.30 );
        background: rgba( 255, 255, 255, 0.25 );
        box-shadow: 0 8px 32px 0 rgba( 31, 38, 135, 0.37 );
        text-shadow: 0 2px 4px rgb(71 97 206 / 36%);
    } 

    li p{
        margin: 10px 20px 10px 20px;
        font-weight: 300;
        align-items: center;
        display: flex;
    }

    li p p:first-child{
        margin: 10px 20px 10px 20px;
        font-weight: 300;
        align-items: center;
        display: flex;
        font-weight: bold;
    }

</style>