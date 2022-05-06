<script>
    export let setDoc;

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
</script>

<div>
    <button on:click={addTodo}>Reset</button>
</div>

<style>
    div {
        position: fixed;
        bottom: 0;
        right: 0;
    }
</style>
