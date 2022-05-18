<script>
    import Color from "./subroutes/color.svelte";
    import Zoom from "./subroutes/zoom.svelte";
    import Canvas from "./subroutes/canvas.svelte";
    import { firebaseConfig } from "./firebase/firebaseconfig";
    import { initializeApp, getApps, getApp } from "firebase/app";
    import { getFirestore, collection, onSnapshot } from "firebase/firestore";

    const firebaseApp =
        getApps().length === 0 ? initializeApp(firebaseConfig) : getApp();

    const db = getFirestore();

    const colRef = collection(db, "pixels");

    let array = [];
    for (let i = 0; i < 150; i++) {
        for (let j = 0; j < 70; j++) {
            array.push({ x: i, y: j, color: "" });
        }
    }
    let morn = [];
    let print = [];
    let hei = false;

    const unsubscribe = onSnapshot(colRef, (querySnapshot) => {
        let pixels = [];
        querySnapshot.forEach((doc) => {
            let pixels_temp = { ...doc.data(), id: doc.id };
            pixels = [pixels_temp, ...pixels];
        });
        pixels = pixels;
        if (pixels[0] != undefined) {
            hei = true;
            morn = pixels[0].pixels;
            for (let i = 0; i < array.length; i++) {
                array[i].color = morn[i];
            }
            print = array;
        }
    });

    let seconds = 0;
    let minutes = 0;
    let dato_boolean = false;
    let countDownDate = localStorage.time != "" ? localStorage.time : "";
    //If there is a time-limit allready
    let x = setInterval(function () {
        timer();
    }, 1000);

    //When canvas.svelte changes a color, seconds is set to 10 and this will execute
    let time = localStorage.time;

    $: if (seconds === 10 || time != localStorage.time) {
        time = localStorage.time;
        dato_boolean = true;
        x = setInterval(function () {
            timer();
        }, 1000);
    }

    function timer() {
        countDownDate =
            localStorage.time != null
                ? localStorage.time
                : (countDownDate = new Date().getTime() - 1);
        if (!countDownDate) {
            clearInterval(x);
            dato_boolean = false;
            return;
        }
        const now = new Date().getTime();
        const distance = countDownDate - now;
        if (distance < 0) {
            clearInterval(x);
            dato_boolean = false;
            return;
        }
        seconds = Math.floor((distance % (1000 * 60)) / 1000);
    }

    let farge = "white";

    //Zooming with mousewheel etc.
    let el = 1;
    let scrollerDisplay = true;
    let border = "none";

    $: if (el > 1) {
        border = "1px solid #ddd";
    } else {
        border = "none";
    }
</script>

<svelte:head
    ><meta
        name="viewport"
        content="width=device-width, initial-scale=0.7, maximum-scale=3.0,  minimum-scale=0.7, user-scalable=0"
    /></svelte:head
>
{#if !hei}
    <p id="loading">Loading...</p>
{:else}
    <div class="hei">
        {#if scrollerDisplay}
            <div class="scroller" />
        {/if}
        <Canvas
            {print}
            {el}
            {border}
            {farge}
            {dato_boolean}
            bind:morn
            bind:countDownDate
            bind:seconds
        />
    </div>
{/if}
{#if dato_boolean}
    <div style="width: 300px; margin: auto;">
        <div class="timebar">
            <span>{minutes}m {seconds}s</span>
        </div>
    </div>
{/if}

<Color bind:farge />

<Zoom bind:el bind:scrollerDisplay />

<style>
    * {
        box-sizing: border-box;
    }
    :global(body) {
        margin: 0;
        padding: 0;
        background-color: black;
    }
    #loading {
        position: absolute;
        top: 4rem;
        font-size: 3rem;
    }
    .scroller {
        height: 75rem;
        width: 151rem;
        position: absolute;
    }
    .hei {
        position: relative;
        top: 1rem;
        left: 1rem;
    }

    .timebar {
        overflow: hidden;
        position: fixed;
        background-color: rgba(51, 51, 51, 0.9);
        left: calc(50% - 6rem);
        top: 4.125rem;
        width: 12rem;
        height: 3.5rem;
        justify-content: center;
        display: flex;
        margin: auto;
    }
    .timebar span {
        color: red;
        font-size: 3rem;
        float: left;
        display: block;
        border: none;
        text-align: center;
        text-decoration: none;
        position: absolute;
        top: -0.625rem;
    }
</style>
