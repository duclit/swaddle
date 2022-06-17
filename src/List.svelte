<!--From https://github.com/brunomolteni/svelte-sortable-list/blob/master/SortableList.svelte with some changes-->

<script>
    import { quintOut } from "svelte/easing";
    import { crossfade } from "svelte/transition";
    import { flip } from "svelte/animate";
    import { createEventDispatcher } from "svelte";
    
    const [send, receive] = crossfade({
        duration: d => Math.sqrt(d * 200),
        fallback(node, params) {
            const style = getComputedStyle(node);
            const transform = style.transform === "none" ? "" : style.transform;
            
            return {
                duration: 600,
                easing: quintOut,
                css: t => `transform: ${transform} scale(${t}); opacity: ${t}`
            };
        }
    });
    
    let isOver = false;
    const getDraggedParent = node =>  node.dataset && node.dataset.index
        ? node.dataset
        : getDraggedParent(node.parentNode);

    const start = ev => {
        ev.dataTransfer.setData("source", ev.target.dataset.index);
    };

    const over = ev => {
        ev.preventDefault();
        let dragged = getDraggedParent(ev.target);
        // if (isOver !== dragged.id) isOver = JSON.parse(dragged.id);
    };

    const leave = ev => {
        let dragged = getDraggedParent(ev.target);
        if (isOver === dragged.id) isOver = false;
    };

    const drop = ev => {
        isOver = false;
        ev.preventDefault();
        let dragged = getDraggedParent(ev.target);
        let from = ev.dataTransfer.getData("source");
        let to = dragged.index;
        reorder({ from, to });
        onDrop();
    };
    
    const dispatch = createEventDispatcher();
    const reorder = ({ from, to }) => {
        let newList = [...list];
        newList[from] = [newList[to], (newList[to] = newList[from])][0];
        dispatch("sort", newList);
    };
    
    const getKey = item => (key ? item[key] : item);
    
    export let list;
    export let key;
    export let onDrop;
</script>
  
<style>
    ul {
        list-style: none;
        padding: 0;

        display: flex;
        flex-direction: column;
        gap: 12px;
    }
</style>
  
<ul id="code">
    {#each list as item, index (getKey(item))}
        <li
            data-index={index}
            data-id={JSON.stringify(getKey(item))}
            draggable="true"
            on:dragstart={start}
            on:dragover={over}
            on:dragleave={leave}
            on:drop={drop}
            in:receive={{ key: getKey(item) }}
            out:send={{ key: getKey(item) }}
            animate:flip={{ duration: 300 }}
            class:over={getKey(item) === isOver}>
            <slot {item} {index}>
                <svelte:component this={item.component}/>
            </slot>
        </li>
    {/each}
</ul>