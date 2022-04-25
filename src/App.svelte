<script>
    import { subscribe } from "svelte/internal";
    import Animation from "./libs/animation.svelte";

    import { fade, scale, slide } from "svelte/transition";
    import { flip } from "svelte/animate";

    let scoops = 0;
    let yes = true;
    let urlGenerator =
        "https://fabienbounoir.github.io/Bouns-Interaction-Twitch-Tchat/";

    let chaine = [];
    let subCustom = [];
    let subGiftCustom = [];
    let cheers = [];
    let avatar = [];
    let backlistUser = [];

    let chaineValue = "";
    let subCustomValue = "";
    let subGiftCustomValue = "";
    let cheersValue = "";
    let avatarValue = "";
    let config = {
        message: false,
        maxdelete: 8,
        timemessage: 1000,
        save: false,
        subscription: false,
        deleted: false,
        subgift: false,
        cheer: false,
        ban: false,
        timeout: false,
        theme: "white",
        avatar: false,
        left: 0,
        animSub: false,
        animSubGift: false,
        animCheer: false,
    };

    var r = document.querySelector(":root");
    //random color hexa except dark colour
    r.style.setProperty(
        "--accentuationColor",
        "#" + Math.random().toString(16).slice(2, 8)
    );

    function addChaine(value) {
        if (value.length == 0) return;
        if (chaine.indexOf(value) !== -1) return;

        chaine = [...chaine, value];
        chaineValue = "";
    }

    function removeChaine(value) {
        chaine = chaine.filter((items) => items != value);
    }

    function removeSub(value) {
        subCustom = subCustom.filter((items) => items != value);
    }

    function addSub(value) {
        if (
            !/^(?:http(s)?:\/\/)?[\w.-]+(?:\.[\w\.-]+)+[\w\-\._~:/?#[\]@!\$&'\(\)\*\+,;=.]+$/.test(
                value
            )
        )
            return;
        if (value.length == 0) return;
        if (subCustom.indexOf(value) !== -1) return;

        subCustom = [...subCustom, value];
        subCustomValue = "";
    }

    function removeSubGift(value) {
        subGiftCustom = subGiftCustom.filter((items) => items != value);
    }

    function addSubGift(value) {
        if (
            !/^(?:http(s)?:\/\/)?[\w.-]+(?:\.[\w\.-]+)+[\w\-\._~:/?#[\]@!\$&'\(\)\*\+,;=.]+$/.test(
                value
            )
        )
            return;
        if (value.length == 0) return;
        if (subGiftCustom.indexOf(value) !== -1) return;

        subGiftCustom = [...subGiftCustom, value];
        subGiftCustomValue = "";
    }

    function removeCheers(value) {
        cheers = cheers.filter((items) => items != value);
    }

    function addCheers(value) {
        if (
            !/^(?:http(s)?:\/\/)?[\w.-]+(?:\.[\w\.-]+)+[\w\-\._~:/?#[\]@!\$&'\(\)\*\+,;=.]+$/.test(
                value
            )
        )
            return;
        if (value.length == 0) return;
        if (cheers.indexOf(value) !== -1) return;

        cheers = [...cheers, value];
        cheersValue = "";
    }

    function removeAvatar(value) {
        avatar = avatar.filter((items) => items != value);
    }

    function addAvatar(value) {
        if (
            !/^(?:http(s)?:\/\/)?[\w.-]+(?:\.[\w\.-]+)+[\w\-\._~:/?#[\]@!\$&'\(\)\*\+,;=.]+$/.test(
                value
            )
        )
            return;
        if (value.length == 0) return;
        if (avatar.indexOf(value) !== -1) return;

        avatar = [...avatar, value];
        avatarValue = "";
    }

    function removeBlacklistUser(value) {
        backlistUser = backlistUser.filter((items) => items != value);
    }

    function addBlacklistUser(value) {
        if (value.length == 0) return;
        if (backlistUser.indexOf(value.toLowerCase()) !== -1) return;

        backlistUser = [...backlistUser, value.toLowerCase()];
        avatarValue = "";
    }

    function copy() {
        let link = `${urlGenerator}?chaine=${chaine.join()}${
            config.message ? "&message=true" : ""
        }${config.badge ? "&badge=true" : ""}${
            config.maxdelete >= 1 && config.maxdelete <= 100
                ? `&maxdelete=${config.maxdelete}`
                : ""
        }${
            !isNaN(config.timemessage)
                ? `&timemessage=${config.timemessage}`
                : ""
        }${config.save ? `&save=true` : ""}${
            config.subscription ? "&subscription=true" : ""
        }${config.deleted ? "&deleted=true" : ""}${
            config.subgift ? "&subgift=true" : ""
        }${config.cheer ? "&cheer=true" : ""}${config.ban ? "&ban=true" : ""}${
            config.timeout ? "&timeout=true" : ""
        }&theme=${config.theme}${config.avatar ? "&avatar=true" : ""}${
            config.left ? "&left=true" : ""
        }${config.animSub ? "&animsub=true" : ""}${
            config.animSubGift ? "&animsubgift=true" : ""
        }${config.animCheer ? "&animcheer=true" : ""}${
            backlistUser.length > 0 ? `&backlist=${backlistUser.join()}` : ""
        }${subCustom.length ? `&customSub=${subCustom.join()}` : ""}${
            subGiftCustom.length ? `&customSubGif=${subGiftCustom.join()}` : ""
        }${cheers.length ? `&customCheers=${cheers.join()}` : ""}${
            avatar.length ? `&customAvatar=${avatar.join()}` : ""
        }`;

        navigator.clipboard.writeText(link).then(
            function () {
                console.log("Copie reussi");
            },
            function () {
                console.log("Copie echoué");
            }
        );
    }

    function replaceStr(str, find, replace) {
        for (var i = 0; i < find.length; i++) {
            str = str.replace(new RegExp(find[i], "gi"), replace[i]);
        }
        return str;
    }
</script>

<main>
    <div class="config">
        <h1>Configuration</h1>
        <div class="element">
            <h2>Chaine</h2>
            <div class="input">
                <p>Chaine à écouter:</p>
                <div class="inputComponent">
                    <input
                        class="check"
                        type="text"
                        placeholder="badbounstv"
                        bind:value={chaineValue}
                    /><button
                        class="button"
                        on:click={() => {
                            addChaine(chaineValue);
                        }}>add</button
                    >
                </div>
                <div class="list">
                    {#each chaine as value (value)}
                        <p
                            transition:scale
                            on:click={() => {
                                removeChaine(value);
                            }}
                        >
                            {value}
                        </p>
                    {/each}
                </div>
            </div>
        </div>
        <div class="element">
            <h2>Type</h2>
            <div class="checkBox allBorder">
                <label>
                    <input type="checkbox" bind:checked={config.message} />
                    Messages
                </label>
                {#if config.message}
                    <label transition:scale>
                        <input type="checkbox" bind:checked={config.save} />
                        Garder les messages
                    </label>
                {/if}
                {#if config.message}
                    <label transition:scale>
                        <input type="checkbox" bind:checked={config.badge} />
                        Badge
                    </label>
                {/if}
            </div>
            {#if config.message}
                <div transition:slide class="checkBox topBorder">
                    <label class="labelDiv"> Nombre de message max </label>
                </div>
                <div transition:slide class="inputComponentNumber">
                    <input
                        class="check"
                        type="number"
                        placeholder="10"
                        min="1"
                        max="100"
                        bind:value={config.maxdelete}
                    />
                </div>
            {/if}
            <div class="checkBox topBorder">
                <label class="labelDiv">
                    <input type="checkbox" bind:checked={config.subscription} />
                    Sub
                </label>
                {#if config.subscription}
                    <label class="labelDiv">
                        <input type="checkbox" bind:checked={config.animSub} />
                        Animation
                    </label>
                {/if}
            </div>
            <div transition:slide class="inputComponent">
                <input
                    class="check"
                    type="text"
                    disabled={!config.subscription || !config.animSub}
                    placeholder="Lien url media"
                    bind:value={subCustomValue}
                /><button
                    class="button"
                    on:click={() => {
                        addSub(subCustomValue);
                    }}>add</button
                >
            </div>
            <div class="list">
                {#each subCustom as value (value)}
                    <p
                        transition:scale
                        on:click={() => {
                            removeSub(value);
                        }}
                    >
                        {value.substr(
                            value.substr(0, 4080).lastIndexOf("/"),
                            value.length
                        )}
                    </p>
                {/each}
            </div>
            <div class="checkBox topBorder">
                <label class="labelDiv">
                    <input type="checkbox" bind:checked={config.subgift} />
                    Sub Gift
                </label>
                {#if config.subgift}
                    <label class="labelDiv">
                        <input
                            type="checkbox"
                            bind:checked={config.animSubGift}
                        />
                        Animation
                    </label>
                {/if}
            </div>
            <div transition:slide class="inputComponent">
                <input
                    class="check"
                    type="text"
                    disabled={!config.subgift || !config.animSubGift}
                    placeholder="Lien url media"
                    bind:value={subGiftCustomValue}
                /><button
                    class="button"
                    on:click={() => {
                        addSubGift(subGiftCustomValue);
                    }}>add</button
                >
            </div>
            <div class="list">
                {#each subGiftCustom as value (value)}
                    <p
                        transition:scale
                        on:click={() => {
                            removeSubGift(value);
                        }}
                    >
                        {value.substr(
                            value.substr(0, 4080).lastIndexOf("/"),
                            value.length
                        )}
                    </p>
                {/each}
            </div>
            <div class="checkBox topBorder">
                <label class="labelDiv">
                    <input type="checkbox" bind:checked={config.cheer} />
                    Cheer
                </label>
                {#if config.cheer}
                    <label class="labelDiv">
                        <input
                            type="checkbox"
                            bind:checked={config.animCheer}
                        />
                        Animation
                    </label>
                {/if}
            </div>
            <div transition:slide class="inputComponent">
                <input
                    class="check"
                    type="text"
                    disabled={!config.cheer || !config.animCheer}
                    placeholder="Lien url media"
                    bind:value={cheersValue}
                /><button
                    class="button"
                    on:click={() => {
                        addCheers(cheersValue);
                    }}>add</button
                >
            </div>
            <div class="list">
                {#each cheers as value (value)}
                    <p
                        transition:scale
                        on:click={() => {
                            removeCheers(value);
                        }}
                    >
                        {value.substr(
                            value.substr(0, 4080).lastIndexOf("/"),
                            value.length
                        )}
                    </p>
                {/each}
            </div>
        </div>

        <div class="element">
            <h2>Avatar</h2>
            <div class="checkBox topBorder">
                <label class="labelDiv">
                    <input type="checkbox" bind:checked={config.avatar} />
                    Avatar
                </label>
            </div>
            <div transition:slide class="inputComponent">
                <input
                    class="check"
                    type="text"
                    disabled={!config.avatar}
                    placeholder="Lien url media"
                    bind:value={avatarValue}
                /><button
                    class="button"
                    on:click={() => {
                        addAvatar(avatarValue);
                    }}>add</button
                >
            </div>
            <div class="list">
                {#each avatar as value (value)}
                    <p
                        transition:scale
                        on:click={() => {
                            removeAvatar(value);
                        }}
                    >
                        {value.substr(
                            value.substr(0, 4080).lastIndexOf("/"),
                            value.length
                        )}
                    </p>
                {/each}
            </div>
        </div>

        <div class="element">
            <h2>Style</h2>
            <div class="gridCenter">
                <!-- <div class="checkBox allBorder">
                    <label>
                        <input type=checkbox bind:checked={config.dark}>
                        Dark mode
                    </label>
                </div> -->

                <div class="marginBottom checkBox allBorder">
                    <label>
                        <input
                            type="radio"
                            bind:group={config.theme}
                            name="theme"
                            value="white"
                        />
                        White
                    </label>

                    <label>
                        <input
                            type="radio"
                            bind:group={config.theme}
                            name="theme"
                            value="dark"
                        />
                        Dark
                    </label>

                    <label>
                        <input
                            type="radio"
                            bind:group={config.theme}
                            name="theme"
                            value="rgb"
                        />
                        RGB
                    </label>

                    <label>
                        <input
                            type="radio"
                            bind:group={config.theme}
                            name="theme"
                            value="flatwhite"
                        />
                        Flat White
                    </label>

                    <label>
                        <input
                            type="radio"
                            bind:group={config.theme}
                            name="theme"
                            value="flatdark"
                        />
                        Flat Dark
                    </label>

                    <label>
                        <input
                            type="radio"
                            bind:group={config.theme}
                            name="theme"
                            value="linearrgb"
                        />
                        linear RGB
                    </label>

                    <label>
                        <input
                            type="radio"
                            bind:group={config.theme}
                            name="theme"
                            value="bluepurple"
                        />
                        bleu violet
                    </label>
                </div>
            </div>
        </div>

        <div class="element">
            <h2>Position</h2>
            <div class="gridCenter">
                <div class="marginBottom checkBox allBorder">
                    <label>
                        <input
                            type="radio"
                            bind:group={config.left}
                            name="scoops"
                            value={1}
                        />
                        Left
                    </label>

                    <label>
                        <input
                            type="radio"
                            bind:group={config.left}
                            name="scoops"
                            value={0}
                        />
                        Right
                    </label>
                </div>
            </div>
        </div>

        <div class="element">
            <h2>Moderation</h2>
            <div class="gridCenter">
                <div class="marginBottom checkBox allBorder">
                    <label>
                        <input type="checkbox" bind:checked={config.ban} />
                        ban
                    </label>
                    <label>
                        <input type="checkbox" bind:checked={config.timeout} />
                        Time out
                    </label>
                    <label>
                        <input type="checkbox" bind:checked={config.deleted} />
                        deleted
                    </label>
                </div>
            </div>
        </div>

        <div class="element">
            <h2>blacklist User</h2>
            <div class="input">
                <p>User a blacklist</p>
                <div class="inputComponent">
                    <input
                        class="check"
                        type="text"
                        placeholder="Username"
                        bind:value={chaineValue}
                    /><button
                        class="button"
                        on:click={() => {
                            addBlacklistUser(chaineValue);
                        }}>add</button
                    >
                </div>
                <div class="list">
                    {#each backlistUser as value (value)}
                        <p
                            transition:scale
                            on:click={() => {
                                removeBlacklistUser(value);
                            }}
                        >
                            {value}
                        </p>
                    {/each}
                </div>
            </div>
        </div>

        {#if chaine.length > 0}
            <div class="buttonlink">
                <button class="generate" on:click={copy}>Copy Link</button>
                <a
                    target="_blank"
                    href={`${urlGenerator}?chaine=${chaine.join()}${
                        config.message ? "&message=true" : ""
                    }${config.badge ? "&badge=true" : ""}${
                        config.subscription ? "&subscription=true" : ""
                    }${
                        config.maxdelete >= 1 && config.maxdelete <= 100
                            ? `&maxdelete=${config.maxdelete}`
                            : ""
                    }${
                        !isNaN(config.timemessage)
                            ? `&timemessage=${config.timemessage}`
                            : ""
                    }${config.save ? `&save=true` : ""}${
                        config.deleted ? "&deleted=true" : ""
                    }${config.subgift ? "&subgift=true" : ""}${
                        config.cheer ? "&cheer=true" : ""
                    }${config.ban ? "&ban=true" : ""}${
                        config.timeout ? "&timeout=true" : ""
                    }&theme=${config.theme}${
                        config.avatar ? "&avatar=true" : ""
                    }${config.left ? "&left=true" : ""}${
                        backlistUser.length > 0
                            ? `&backlist=${backlistUser.join()}`
                            : ""
                    }${config.animSub ? "&animsub=true" : ""}${
                        config.animSubGift ? "&animsubgift=true" : ""
                    }${config.animCheer ? "&animcheer=true" : ""}${
                        subCustom.length ? `&customSub=${subCustom.join()}` : ""
                    }${
                        subGiftCustom.length
                            ? `&customSubGif=${subGiftCustom.join()}`
                            : ""
                    }${cheers.length ? `&customCheers=${cheers.join()}` : ""}${
                        avatar.length ? `&customAvatar=${avatar.join()}` : ""
                    }`}><button class="generate">Open Link</button></a
                >
            </div>
        {/if}
    </div>
    <div class="visuel">
        <Animation />
    </div>
</main>

<style>
    :root {
        --accentuationColor: rgb(149, 0, 255);
    }

    main {
        width: 100%;
        display: grid;
        grid-template-columns: 2fr 1fr;
    }

    h1 {
        color: var(--accentuationColor);
        text-align: center;
    }

    .buttonlink {
        display: flex;
        gap: 1em;
    }

    .buttonlink a {
        width: 100%;
    }

    .generate {
        padding: 4px 6px 4px 6px;
        background: var(--accentuationColor);

        border-radius: 12px;
        border-radius: 0.375rem 0.375rem 0.375rem 0.375rem;
        border: 0px;
        width: 100%;
        padding: 10px 0px 10px 0px;
        font-weight: 600;
        font-size: 1.25rem;
        margin-top: 1rem;
        transition: transform 430ms ease-in-out;
        color: black;
        /* margin-top: 1rem; */
        /* margin: 10px 10px 10px 0px; */
    }

    .generate:hover {
        padding: 4px 6px 4px 6px;
        background: rgb(183, 82, 255);

        border-radius: 12px;
        border-radius: 0.375rem 0.375rem 0.375rem 0.375rem;
        border: 0px;
        width: 100%;
        padding: 10px 0px 10px 0px;
        font-weight: 600;
        font-size: 1.25rem;
        margin-top: 1rem;
        transform: scale(1.02);
        /* margin-top: 1rem; */
        /* margin: 10px 10px 10px 0px; */
    }

    h2 {
        color: white;
        margin: 0%;
        text-align: center;
    }

    .config {
        padding: 0.5rem;
        /* width: 50%; */
    }

    :global(body) {
        background-color: #111111;
    }

    .visuel {
        width: 50%;
        /* background-color: blue; */
    }

    .element {
        padding: 1.5rem;
        --tw-bg-opacity: 1;
        background-color: rgba(0, 0, 0, var(--tw-bg-opacity));
        border-radius: 0.5rem;
        margin-top: 1rem;
    }

    .element div {
        --tw-text-opacity: 1;
        color: rgba(0, 0, 0, var(--tw-text-opacity));
    }

    .input {
        margin-top: 1rem;
    }

    input {
        accent-color: red;
    }

    .checkBox {
        margin-top: 1rem;
    }

    .element .checkBox {
        font-weight: 600;
        font-size: 1.25rem;
        line-height: 1.75rem;
        /* --tw-bg-opacity: 1; */
        /* background-color: var(--accentuationColor); */
        display: flex;
        /* width: max-content; */
        /* padding: 10px; */
        /* border-radius: 0.375rem; */
        /* padding-top: 0.5rem;
        padding-bottom: 0.5rem; */
        flex-wrap: wrap;
    }

    .allBorder {
        border-radius: 0.375rem;
    }

    .topBorder {
        border-radius: 0.375rem 0.375rem 0rem 0rem;
    }

    .inputComponent input {
        height: 48px !important;
        font-size: 1.5rem;
        line-height: 2rem;
        padding-top: 0.5rem;
        padding-bottom: 0.5rem;
        padding-left: 0.5rem;
        padding-right: 0.5rem;
        --tw-bg-opacity: 1;
        background-color: rgba(51, 51, 51, var(--tw-bg-opacity));
        color: white;
        border-radius: 0rem 0rem 0rem 0.375rem;
        resize: none;
        /* width: 100%; */
        box-sizing: border-box;
        border: 0ch;
        outline: 0px var(--accentuationColor) solid;
        white-space: nowrap;
        width: inherit;
    }

    .inputComponentNumber input {
        height: 48px !important;
        font-size: 1.5rem;
        line-height: 2rem;
        padding-top: 0.5rem;
        padding-bottom: 0.5rem;
        padding-left: 0.5rem;
        padding-right: 0.5rem;
        --tw-bg-opacity: 1;
        background-color: rgba(51, 51, 51, var(--tw-bg-opacity));
        color: white;
        border-radius: 0rem 0.375rem 0.375rem 0.375rem;
        resize: none;
        /* width: 100%; */
        box-sizing: border-box;
        border: 0ch;
        outline: 0px var(--accentuationColor) solid;
        white-space: nowrap;
        width: inherit;
    }

    .inputComponentNumber {
        width: 100%;
        align-items: center;
        display: flex;
    }

    input:disabled {
        background-color: rgba(30, 30, 30, var(--tw-bg-opacity));
    }

    .inputComponent {
        width: 100%;
        align-items: center;
        display: flex;
    }

    .inputComponent button {
        height: 48px;
        background-color: var(--accentuationColor);
        border: 0ch;
        border-radius: 0rem 0.375rem 0.375rem 0rem;
        padding-top: 0.5rem;
        padding-bottom: 0.5rem;
        padding-left: 0.5rem;
        padding-right: 0.5rem;
        font-weight: 500;
        font-size: 1.25rem;
        margin-left: 2px;
        color: black;
    }

    .inputComponent button:active {
        height: 48px;
        background-color: rgba(256, 211, 214, var(--tw-bg-opacity));
        border: 0ch;
        border-radius: 0rem 0.375rem 0.375rem 0rem;
        padding-top: 0.5rem;
        padding-bottom: 0.5rem;
        padding-left: 0.5rem;
        padding-right: 0.5rem;
        font-weight: 500;
        font-size: 1.25rem;
    }

    .input p {
        margin: 0%;
        background-color: var(--accentuationColor);
        width: max-content;
        padding: 10px;
        border-radius: 0.375rem 0.375rem 0rem 0rem;
        padding-top: 0.5rem;
        padding-bottom: 0.5rem;
        font-weight: 600;
        font-size: 1.25rem;
    }

    .list p {
        margin: 0%;
        background-color: var(--accentuationColor);
        width: max-content;
        padding: 10px;
        border-radius: 0.375rem 0.375rem 0rem 0rem;
        padding-top: 0.5rem;
        padding-bottom: 0.5rem;
        font-weight: 600;
        font-size: 1.25rem;
    }

    .list {
        display: flex;
        gap: 0.5em;
        flex-wrap: wrap;
        border-bottom: 0px solid black;
        margin-top: 1rem;
        /* display: grid;
        grid-template-columns: repeat(5, auto) */
    }

    .list p {
        padding: 4px 6px 4px 6px;
        background: var(--accentuationColor);

        border-radius: 12px;
        border-radius: 0.375rem 0.375rem 0.375rem 0.375rem;
        /* margin-top: 1rem; */
        /* margin: 10px 10px 10px 0px; */
    }

    .list p:hover {
        background: #7b0000;
    }

    .checkBox .labelDiv {
        /* border-left: 2px black solid; */
        margin-right: 10px;
        font-weight: 600;
        font-size: 1.25rem;
        line-height: 1.75rem;
        --tw-bg-opacity: 1;
        background-color: var(--accentuationColor);
        width: max-content;
        padding: 10px;
        border-radius: 0.375rem 0.375rem 0rem 0rem;
        padding-top: 0.5rem;
        padding-bottom: 0.5rem;
    }

    .checkBox label {
        /* border-left: 2px black solid; */
        margin-right: 10px;
        font-weight: 600;
        font-size: 1.25rem;
        line-height: 1.75rem;
        --tw-bg-opacity: 1;
        background-color: var(--accentuationColor);
        width: max-content;
        padding: 10px;
        border-radius: 0.375rem;
        padding-top: 0.5rem;
        padding-bottom: 0.5rem;
    }

    .marginBottom label {
        margin-bottom: 10px;
    }

    .gridCenter {
        display: grid;
        grid-template-columns: 1fr 1fr 1fr;
        justify-items: center;
    }

    .gridCenterTwo {
        display: grid;
        grid-template-columns: 1fr 1fr;
        justify-items: center;
    }

    .gridCenter {
        display: grid;
        grid-template-columns: 1fr;
        justify-items: center;
    }

    * {
        box-sizing: border-box;
        /* font-family: proxima-nova; */
    }

    @media screen and (max-width: 1510px) {
        main {
            width: 100%;
            display: grid;
            grid-template-columns: 1.4fr 1fr;
        }
    }

    @media screen and (max-width: 980px) {
        main {
            width: 100%;
            display: grid;
            grid-template-columns: 1fr;
        }

        .visuel {
            display: none;
        }

        .config {
            width: 100%;
        }
    }

    @media screen and (max-width: 440px) {
        .gridCenter {
            grid-template-columns: 1fr;
        }

        .marginBottom label {
            width: -webkit-fill-available;
        }
    }
</style>
