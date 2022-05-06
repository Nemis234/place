<script>
    import { initializeApp, getApps, getApp } from "firebase/app";
    import {
        getFirestore,
        collection,
        onSnapshot,
        doc,
        updateDoc,
        setDoc,
    } from "firebase/firestore";
    /* import { browser } from "$app/env"; */

    const browser = true;

    const firebaseConfig = {
        apiKey: "AIzaSyAInJ3jBubdQhyS4lSDhgs-2fb-DA3n2v8",
        authDomain: "sveltekit-todos-b95d8.firebaseapp.com",
        projectId: "sveltekit-todos-b95d8",
        storageBucket: "sveltekit-todos-b95d8.appspot.com",
        messagingSenderId: "301130026504",
        appId: "1:301130026504:web:558d1541faf4184fde8bbd",
    };

    const firebaseApp =
        browser &&
        (getApps().length === 0 ? initializeApp(firebaseConfig) : getApp());

    const db = browser && getFirestore();

    const colRef = browser && collection(db, "pixels");

    let array = [];
    for (let i = 0; i < 150; i++) {
        for (let j = 0; j < 70; j++) {
            array.push({ x: i, y: j, color: "" });
        }
    }
    let pixels = [];
    const unsubscribe =
        browser &&
        onSnapshot(colRef, (querySnapshot) => {
            let fbTodos = [];
            querySnapshot.forEach((doc) => {
                let pixels = { ...doc.data(), id: doc.id };
                fbTodos = [pixels, ...fbTodos];
            });
            pixels = fbTodos;
            if (pixels[0] != undefined) {
                hei = true;
                morn = pixels[0].pixels;
                for (let i = 0; i < array.length; i++) {
                    array[i].color = morn[i];
                }
                print = array;
            }
        });

    let morn = [];
    let hei = false;
    let print = [];

    function reset() {
        let array_reset = {};
        for (let i = 0; i < array.length; i++) {
            let hi = { [i]: "#ffffff" };
            Object.assign(array_reset, hi);
        }

        return array_reset;
    }

    const addTodo = async () => {
        let array_reset = reset();
        await setDoc(doc(db, "pixels", "pixel"), {
            pixels: array_reset,
        });
    };

    let farge = "white";
    let c = ["#ddd"];

    const fargeendring = (a, i) => {
        farge = a;
        for (let i = 0; i < c.length; i++) {
            c[i] = null;
        }
        c[i] = "#ddd";
    };

    const endre_farge = async (i) => {
        if (dato_boolean) {
            return;
        }
        dato_boolean = true;
        morn[i] = farge;
        countDownDate = new Date().getTime() + 11000;

        localStorage.time = countDownDate;
        x = setInterval(function () {
            timer();
        }, 1000);
        minutes = 0;
        seconds = 10;
        await updateDoc(doc(db, "pixels", "pixel"), {
            ["pixels." + i]: farge,
        });
    };
    var countDownDate =
        localStorage.time != ""
            ? localStorage.time
            : (countDownDate = new Date().getTime());

    var seconds;
    var minutes = 0;
    let dato_boolean = false;

    var x = setInterval(function () {
        timer();
    }, 1000);

    function timer() {
        dato_boolean = true;
        countDownDate =
            localStorage.time != null
                ? localStorage.time
                : (countDownDate = new Date().getTime() - 1);
        if (countDownDate == null) {
            clearInterval(x);
            dato_boolean = false;
            return;
        }
        var now = new Date().getTime();

        var distance = countDownDate - now;
        if (distance < 0) {
            clearInterval(x);
            dato_boolean = false;
            return;
        }

        /* minutes = Math.floor((distance % (1000 * 60 * 60)) / (1000 * 60)); */
        seconds = Math.floor((distance % (1000 * 60)) / 1000);
    }

    let el = 1;
    let interval = false;

    const handleWheel = (e) => {
        if (e.ctrlKey) {
            console.log(interval + "hi");
            if (interval) {
                e.preventDefault();
                return;
            }
            interval = true;
            console.log(interval);
            if (e.deltaY * -1 < 0) {
                zoom(false);
            } else {
                zoom(true);
            }
            /* scale += e.deltaY * -0.001;
            // Restrict scale
            scale = Math.min(Math.max(0.5, scale), 4);
            el = scale; */

            setTimeout(function () {
                interval = false;
            }, 500);
            e.preventDefault();
        }
    };
    let scrollerDisplay = true;
    const zoom = (zoom) => {
        if (zoom) {
            if (el < 1) {
                el = 1;
                scrollerDisplay = true;
            } else {
                el = 4;
                scrollerDisplay = false;
            }
        } else {
            if (el <= 1) {
                el = 0.5;
                scrollerDisplay = false;
            } else {
                el = 1;
                scrollerDisplay = true;
            }
        }
    };

    window.addEventListener("mousewheel", handleWheel, { passive: false });
</script>

<svelte:head
    ><meta
        name="viewport"
        content="width=device-width, initial-scale=0.8, maximum-scale=3.0,  minimum-scale=0.8"
    /></svelte:head
>

{#if !hei}
    <p id="loading">Loading</p>
{:else}
    <div class="hei">
        {#if scrollerDisplay}
            <div class="scroller" />
        {/if}

        {#each print as item, i}
            <span
                class="pixl"
                style="background-color: {item.color}; left:{item.x *
                    el}rem; top: {item.y * el + 4}rem; transform: scale({el});"
                on:click={() => endre_farge(i)}
            />
        {/each}
    </div>
{/if}
{#if dato_boolean}
    <div style="width: 300px; margin: auto;">
        <div class="timebar">
            <span>{minutes}m {seconds}s</span>
        </div>
    </div>
{/if}
<div class="navbar">
    <!-- <button on:click={addTodo}>Reset</button> -->
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
<div class="zoom-controls">
    <div id="zoom-in" on:click={() => zoom(true)}>+</div>
    <div id="zoom-out" on:click={() => zoom(false)}>-</div>
</div>

<style>
    * {
        box-sizing: border-box;
    }
    .zoom-controls {
        position: fixed;
        padding: 2px;
        border-radius: 5px;
        background-color: rgba(256, 256, 256, 0.9);
        top: 5.5rem;
        right: 2rem;
    }
    .zoom-controls div {
        background-color: rgba(0, 0, 0, 0.7);
        padding: 10px;
        border-radius: 5px;
        color: white;
        font-size: 12px;
    }
    .zoom-controls div:hover {
        cursor: pointer;
        background-color: black;
    }

    .zoom-controls div:not(:first-child) {
        margin-top: 5px;
    }
    :global(body) {
        margin: 0;
        padding: 0;
        background-color: black;
    }
    #loading {
        position: absolute;
        top: 4rem;
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
    .pixl {
        width: 1rem;
        height: 1rem;
        margin: none;
        border-collapse: separate;
        background-color: none;
        position: absolute;
        box-sizing: border-box;
    }
    .pixl:hover {
        border: 1px solid black;
    }
    .timebar {
        overflow: hidden;
        position: fixed;
        background-color: rgba(51, 51, 51, 0.9);
        left: (50% - 6rem);
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

    .navbar button {
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
    .navbar button:hover {
        background: #ddd;
        color: black;
    }
</style>
