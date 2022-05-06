<script>
    import { initializeApp, getApps, getApp } from "firebase/app";
    import {
        getFirestore,
        collection,
        onSnapshot,
        doc,
        addDoc,
        updateDoc,
        deleteDoc,
        setDoc,
        arrayUnion,
        arrayRemove,
        writeBatch,
        FieldValue,
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
        });

    let array = [];
    for (let i = 0; i < 150; i++) {
        for (let j = 0; j < 70; j++) {
            array.push({ x: i, y: j, color: "" });
        }
    }

    let morn = [];
    let hei = false;
    let print = [];

    $: if (pixels[0] != undefined) {
        hei = true;
        morn = pixels[0].pixels;
        for (let i = 0; i < array.length; i++) {
            array[i].color = morn[i];
        }
        print = array;
    }

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

    let farge = "blue";
    let c = ["#ddd"];

    const fargeendring = (a, i) => {
        farge = a;
        for (let i = 0; i < 5; i++) {
            c[i] = null;
        }
        c[i] = "#ddd";
    };

    const endre_farge = async (i) => {
        console.log(dato_boolean);
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
        console.log(dato_boolean);
        if (countDownDate == null) {
            clearInterval(x);
            dato_boolean = false;
            return;
        }
        var now = new Date().getTime();

        var distance = countDownDate - now;
        console.log(distance);
        if (distance < 0) {
            console.log("hei");
            clearInterval(x);
            dato_boolean = false;
            return;
        }

        /* minutes = Math.floor((distance % (1000 * 60 * 60)) / (1000 * 60)); */
        seconds = Math.floor((distance % (1000 * 60)) / 1000);
    }
    /* 
    var y = window.matchMedia("(max-width: 700px)");
    console.log(y); // Call listener function at run time
    function myFunction() {
        console.log(y);
    }
    y.addListener(myFunction); // Attach listener function on state changes */
</script>

<svelte:head
    ><meta
        name="viewport"
        content="width=device-width, initial-scale=0.8, maximum-scale=3.0,  minimum-scale=0.8"
    /></svelte:head
>

<svelte:body />

{#if !hei}
    <p id="loading">Loading</p>
{:else}
    <div class="hei">
        {#each print as item, i}
            <div
                class="pixl"
                style="background-color: {item.color}; left:{item.x}rem; top: {item.y +
                    4}rem;"
                on:click={() => endre_farge(i)}
            />
        {/each}
    </div>
{/if}
{#if dato_boolean}
    <div class="timebar">
        <p>{minutes}m {seconds}s</p>
    </div>
{/if}
<div class="navbar">
    <!-- <button on:click={addTodo}>Reset</button> -->
    <button
        style="background-color: {c[0]}; color: blue;"
        on:click={() => fargeendring("blue", 0)}>Blå</button
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

<!-- <div style="position:relative; width:2560px; height: 1280px; top: 100px;" /> -->
<style>
    :global(body) {
        margin: 0;
        padding: 0;
        background-color: black;
    }
    #loading {
        position: absolute;
        top: 4rem;
    }
    .hei {
        position: relative;
        top: 10px;
        left: 10px;
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
        top: 0;
        width: 100%;
        height: 10rem;
        text-align: center;
        display: flex;
        justify-content: center;
    }
    .timebar p {
        color: red;
        font-size: 3rem;
        float: left;
        display: block;
        border: none;
        text-align: center;
        padding: 14px 16px;
        text-decoration: none;
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
