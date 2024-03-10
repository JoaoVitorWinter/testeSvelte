<script>
// @ts-nocheck
    import { fly, fade } from 'svelte/transition';
    import { flip } from 'svelte/animate';
    let test = true;
    let list = [];
    let valueInput = "";
    let quantityNeeded = null;
    $: disabled = !(valueInput != "" && quantityNeeded != null);

    function handleAdd (event) {
            let newItem = { 
                name: valueInput,
                quantity: quantityNeeded,
                initialQuantity: 0
            }
            list = [...list, newItem];
            valueInput = "";
            quantityNeeded = null;
    }

    function handleRemove (i) {
        var newList = [...list];
        newList.splice(i, 1);
        list = newList;
    }

</script>

<main>
    <input type="checkbox" bind:checked={test}>
    <h1>ToDo List</h1>
    <div id="form">
        <label for="title">
        <input id="title" bind:value={valueInput}>
        </label> 
        <label for="quantity">
        <input id="quantity" type="number" bind:value={quantityNeeded}>
        </label>  
        <button id="buttonAdd" {disabled} on:click={handleAdd}>Adicionar</button>
    </div>

    {#if test}
        
    <section id="list">
        {#each list as item, index (item)}
        <div class="item"
        in:fly={{ y: 200, duration: 500 }}
        out:fly|global={{x: 500, duration: 500}}
        animate:flip={{ duration: 500 }}
        >
            <progress value={item.initialQuantity / item.quantity}></progress>
            <div>
                
                <p class="title">{item.name}</p>
                <div class="counter">
                    <button on:click={() => {
                        if(item.initialQuantity >= 1) {
                            item.initialQuantity -= 1;
                        } 
                    }}>-</button>
                <p>{item.initialQuantity}</p>
                <button on:click={() => {
                    if(item.initialQuantity < item.quantity) {
                        item.initialQuantity += 1;
                    } 
                }}>+</button>
            </div>
            <button class="buttonRemove" on:click={() => handleRemove(index)}>X</button>
        </div>
        </div>
        {/each}
    </section>
    {/if}
</main>

<style>
    #form {
        margin-top: 24px;
        display: flex;
        flex-wrap: wrap;
        gap: 8px;
    }

    input, #buttonAdd {
        padding: 8px 16px;
    }

    .buttonRemove {
        border: none;
    }

    .buttonRemove:hover {
        cursor: pointer;
    }

    #list {
        margin-top: 16px;
        display: flex;
        flex-direction: column;
        gap: 16px;
    }

    .item {
        display: flex;
        flex-direction: column;
        justify-content: space-between;
        align-items: center;
        width: 352px;
        border: 1px solid black;
        box-sizing: border-box;
    }

    progress {
        width: 102%;
        margin-top: -8px;
        border-radius: 0;
        background-color: aqua;
    }

    .item > div {
        width: 100%;
        display: flex;
        justify-content: space-between;
        align-items: center;
        padding: 8px 16px;
        box-sizing: border-box;
    }

    .title {
        width: 80px;
        overflow: hidden;
    }

    .counter {
        display: flex;
        align-items: center;
        gap: 8px;
    }

    .counter button {
        padding: 4px 8px;
    }



</style>