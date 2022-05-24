<script>
    import swal from "sweetalert";

    import { getFirestore, doc, updateDoc } from "firebase/firestore";
    const db = getFirestore();
    export let print, el, border, farge, morn, seconds, minutes, antallSekunder;

    const endre_farge1 = (i) => {
        if (seconds != 0) {
            swal({
                title: `Du må vente i ${seconds} sekunder`,
                icon: "error",
            });
            return;
        }
        swal({
            title: "Vil du plasere pikselen?",
            icon: "warning",
            buttons: ["Nei", "Ja"],
            dangerMode: true,
        }).then((willDelete) => {
            if (willDelete) {
                endre_farge2(i);
            }
        });
    };

    const endre_farge2 = async (i) => {
        morn[i] = farge;

        seconds = Math.floor((antallSekunder % (1000 * 60)) / 1000);
        minutes = Math.floor((antallSekunder % (1000 * 60 * 60)) / (1000 * 60));

        localStorage.time =
            new Date().getTime() + (antallSekunder * 1000 + 1000);
        await updateDoc(doc(db, "pixels", "pixel"), {
            ["pixels." + i]: farge,
        });
    };
</script>

<div class="canvas" style="--el: {el}; --border: {border}">
    {#each print as item, i}
        <span
            class="pixl"
            style="background-color: {item.color}; left:{item.x *
                el}rem; top: {item.y * el + 4}rem;;"
            on:click={() => endre_farge1(i)}
        />
    {/each}
</div>

<style>
    .pixl {
        width: 1rem;
        height: 1rem;
        margin: none;
        border-collapse: separate;
        border: var(--border);
        background-color: none;
        position: absolute;
        box-sizing: border-box;
        transform: scale(var(--el));
    }
    .pixl:hover {
        border: 1px solid black;
    }
</style>
