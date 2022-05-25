<script>
    /*     import Reset from "../firebase/reset.svelte"; */

    export let farge;

    let c = ["#ddd"];
    const fargeendring = (a, i) => {
        farge = a;
        for (let i = 0; i < c.length; i++) {
            c[i] = null;
        }
        c[i] = "#ddd";
    };
    let media = 1;
    function myFunction(x) {
        if (x.matches) {
            media = 0.7;
        } else {
            media = 1;
        }
    }

    var mornsa = window.matchMedia("(max-width: 700px)");
    myFunction(mornsa);
    mornsa.addListener(myFunction);
</script>

<div class="navbar" style="--media: {media}">
    <button
        style="background-color: {c[0]}; color: white"
        on:click={() => fargeendring("white", 0)}>Hvit</button
    >
    <button
        style="background-color: {c[1]}; color: red"
        on:click={() => fargeendring("red", 1)}>Rød</button
    >
    <button
        style="background-color: {c[2]}; color: green"
        on:click={() => fargeendring("green", 2)}>Grønn</button
    >
    <button
        style="background-color: {c[3]}; color: #00FFF0"
        on:click={() => fargeendring("#00FFF0", 3)}>Turkis</button
    >
    <button
        style="background-color: {c[4]}; color: yellow"
        on:click={() => fargeendring("yellow", 4)}>Gul</button
    >
    <button
        style="background-color: {c[5]}; color: blue;"
        on:click={() => fargeendring("blue", 5)}>Blå</button
    >
    <button
        style="background-color: {c[6]}; color: black"
        on:click={() => fargeendring("black", 6)}>Svart</button
    >
    <!-- <Reset {media} /> -->
</div>

<style>
    .navbar {
        overflow: hidden;
        white-space: nowrap;
        display: flex;
        justify-content: center;
        background-color: #333;
        position: fixed;
        top: 0;
        left: 0;
        width: 100%;
        height: 4rem;
    }
    button {
        position: relative;
        color: #f2f2f2;
        background-color: #333;
        border: none;
        margin: none;
        text-align: center;
        height: 100%;
        width: auto;
        text-decoration: none;
        font-size: calc(2rem * var(--media));
    }
    button:hover {
        background: #ddd;
        color: black;
    }
</style>
