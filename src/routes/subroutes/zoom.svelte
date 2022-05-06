<script>
    export let scrollerDisplay;
    export let el;

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
            setTimeout(function () {
                interval = false;
            }, 500);
            e.preventDefault();
        }
    };

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

<div class="zoom-controls">
    <div id="zoom-in" on:click={() => zoom(true)}>+</div>
    <div id="zoom-out" on:click={() => zoom(false)}>-</div>
</div>

<style>
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
</style>
