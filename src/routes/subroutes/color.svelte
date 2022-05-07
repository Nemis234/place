<script>
    export let farge;
    let c = ["#ddd"];
    const fargeendring = (a, i) => {
        farge = a;
        for (let i = 0; i < c.length; i++) {
            c[i] = null;
        }
        c[i] = "#ddd";
    };
</script>

<div class="navbar">
    <button
        style="background-color: {c[0]}; color: white"
        on:click={() => fargeendring("white", 0)}>Hvit</button
    >
    <button
        style="background-color: {c[5]}; color: blue;"
        on:click={() => fargeendring("blue", 5)}>Blå</button
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
</div>

<style>
    @media only screen and (max-width: 500px) {
        button {
            font-size: 1rem;
        }
    }
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
        text-decoration: none;
        font-size: 2rem;
    }
    button:hover {
        background: #ddd;
        color: black;
    }
</style>
