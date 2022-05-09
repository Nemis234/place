<script>
    export let scrollerDisplay;
    export let el;

    let canRun = false;
    let timer;

    const handleWheel = (e) => {
        if (!e.ctrlKey) {
            return;
        }
        e.preventDefault();
        if (canRun) {
            console.log("hei");
            return;
        }
        canRun = true;
        timer = setTimeout(timeout, 300);
        if (e.deltaY > 0) {
            zoom(false);
        } else if (e.deltaY < 0) {
            zoom(true);
        }
    };

    function timeout() {
        console.log("morn");
        canRun = false;
        clearTimeout(timer);
    }

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

    let media = 1;
    function myFunction(x) {
        if (x.matches) {
            media = 1.5;
        } else {
            media = 1;
        }
    }

    var mornsa = window.matchMedia("(max-width: 700px)");
    myFunction(mornsa);
    mornsa.addListener(myFunction);
</script>

<div class="zoom-controls" style="--media: {media}">
    <div id="zoom-in" on:click={() => zoom(true)}>+</div>
    <div id="zoom-out" on:click={() => zoom(false)}>-</div>
    <span>{100 * el}%</span>
</div>

<style>
    .zoom-controls {
        position: fixed;
        padding: calc(3px * var(--media));
        border-radius: 5px;
        background-color: rgba(256, 256, 256, 0.9);
        top: 5.5rem;
        right: 2rem;
    }
    .zoom-controls div {
        text-align: center;
        background-color: rgba(0, 0, 0, 0.7);
        width: auto;
        padding: calc(15px);
        border-radius: 5px;
        color: white;
        font-size: calc(1rem * 2 * var(--media));
    }
    .zoom-controls div:hover {
        cursor: pointer;
        background-color: black;
    }

    .zoom-controls div:not(:first-child) {
        margin-top: 5px;
    }
</style>
