<script>
    import { getFirestore, doc, setDoc } from "firebase/firestore";
    const db = getFirestore();

    export let media;

    function reset() {
        let array_reset = {};
        for (let i = 0; i < array.length; i++) {
            let hi = { [i]: "#fffff" };
            Object.assign(array_reset, hi);
        }
        return array_reset;
    }

    const resetCanvas = async () => {
        let array_reset = reset();
        await setDoc(doc(db, "pixels", "morn"), {
            pixels: array_reset,
        });
    };
</script>

<button style="--media: {media}" on:click={resetCanvas}>Reset</button>

<style>
    button {
        position: relative;
        color: #f2f2f2;
        background-color: red;
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
