<script>
    let matrix = {
        m11: 1,
        m12: 0,
        m13: 0,
        m21: 0,
        m22: 1,
        m23: 0,
        m31: 0,
        m32: 0,
    };

    $: cssVarStyles = Object.entries(matrix)
        .map(([key, value]) => `--${key}:${value}`)
        .join(';');
</script>

<style>
	main {
		text-align: center;
		padding: 1em;
		max-width: 600px;
        display: flex;
	}

    .flex-max {
        flex: 1 0;
        height: 300px;
    }

    #matrix {
        margin: 0 auto;
        padding: 8px;
        background: white;
        z-index: 2;
    }

    #matrix-grid {
        margin-top: 50px;
        padding: 20px;
        border-left: 1px solid black;
        border-right: 1px solid black;

        display: grid;
        grid-template-columns: 1fr 1fr 1fr;
        grid-template-rows: 1fr 1fr 1fr;
        gap: 4px 20px;
        grid-template-areas:
        ". . ."
        ". . ."
        ". . .";
    }

    input {
        width: 100%;
    }

    #canvas {
        border: 1px solid black;
        position: relative;
        z-index: 0;
    }

    #image {
        position: absolute;
        top: 90px;
        left: 90px;
        width: 100px;
        height: 100px;
        transform: matrix3d(
            var(--m11), var(--m21), 0, var(--m31),
            var(--m12), var(--m22), 0, var(--m32),
            0, 0, 1, 0,
            var(--m13), var(--m23), 0, 1
        );
        background-image: url("../swistak.png");
        z-index: 1;
    }
</style>

<main style="{cssVarStyles}">
    <div id="matrix" class="flex-max">
        <div id="matrix-grid">
            {#each Object.keys(matrix) as key}
                <input type="number" bind:value={matrix[key]} step={['m13', 'm23'].includes(key) ? 1 : 0.001}/>
            {/each}
            <input type="number" value={1} disabled/>
        </div>
    </div>
    <div id="canvas" class="flex-max">
        <div id="image"></div>
    </div>
</main>
