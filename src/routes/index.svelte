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

    /* await updateDoc(doc(db, "pixels", "pixel"), {
        regions: arrayUnion("greater_virginia"),
    }); */

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
            let hi = { [i]: "#00FFF0" };
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

    const fargeendring = (a) => {
        farge = a;
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
        seconds = 30;
        /* await updateDoc(doc(db, "pixels", "timestamp"), {
            time: countDownDate,
        }); */
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
            clearInterval(x);
            dato_boolean = false;
            return;
        }

        /* minutes = Math.floor((distance % (1000 * 60 * 60)) / (1000 * 60)); */
        seconds = Math.floor((distance % (1000 * 60)) / 1000);
    }
</script>

<body>
    <div style="position:relative;min-width:960px" />

    <div class="hei">
        {#if !hei}
            <p id="loading">Loading</p>
        {:else}
            {#each print as item, i}
                <div
                    class="pixl"
                    style="background-color: {item.color}; left:{item.x +
                        1.5}rem; top: {item.y + 1.5}rem;"
                    on:click={() => endre_farge(i)}
                />
            {/each}
        {/if}
    </div>

    <div class="navbar">
        <!-- <button on:click={addTodo}>Reset</button> -->
        <button style="color: blue" on:click={() => fargeendring("blue")}
            >Blå</button
        >
        <button style="color: red" on:click={() => fargeendring("red")}
            >Rød</button
        >
        <button style="color: green" on:click={() => fargeendring("green")}
            >Grønn</button
        >
        <button style="color: #00FFF0" on:click={() => fargeendring("#00FFF0")}
            >Turkis</button
        >
    </div>
    {#if dato_boolean}
        <div class="timebar">
            <p>{minutes}m {seconds}s</p>
        </div>
    {/if}
</body>

<style>
    #loading {
        position: absolute;
        top: 4rem;
    }
    .pixl {
        width: 1rem;
        height: 1rem;
        border-collapse: collapse;
        border: 1px solid white;
        background-color: none;
        position: absolute;
    }
    .timebar {
        overflow: hidden;
        position: fixed;
        bottom: 0;
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
        display: flex;
        justify-content: center;
        background-color: #333;
        position: fixed;
        bottom: 0;
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
    .navbar button:target {
        background: #ddd;
        color: black;
    }
    .hei {
        display: block;
        position: absolute;
    }
</style>
