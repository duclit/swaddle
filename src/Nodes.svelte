<script>
    import CallFunction from './Nodes/CallFunction.svelte';
    import Print from './Nodes/Print.svelte';
    import SetVariable from './Nodes/SetVariable.svelte';

    // Function to add an item to the code list. Passed as a prop by the parent component
    export let add;
    export let searchBar = false;
    
    const allNodes = [CallFunction, Print, SetVariable];
    const allNodesMap = {
        'Call_Function': CallFunction,
        'Print': Print,
        'Set_Variable': SetVariable
    }

    let showing = allNodes;
    let self;

    // targetString must be a valid key from allNodesMap. Keywords are generated automatically
    function matches(string, targetString) {
        const keywords = targetString.toLowerCase().split('_');
        const whole = targetString.toLowerCase().replace('_', ' ');

        for (const word of keywords)
            if (word.startsWith(string.trim())) return allNodesMap[targetString];

        if (string === whole || whole.startsWith(string)) return allNodesMap[targetString];
        return null;
    }

    function onKeyup(event) {
        if (event.key === 'Enter') {
            add(showing[0]);
        } else {
            let temp_showing = [];
        
            for (const node of Object.keys(allNodesMap)) {
                const match = matches(self.value, node);
                if (!match) continue;
                temp_showing.push(match);
            }

            showing = temp_showing;

            if (showing.length === 0) {
                // TODO: 'No matches found' label 
            }
        }
    }
</script>

<main>
    {#if searchBar}
        <input type="text" placeholder="Search nodes.." on:keyup={onKeyup} bind:this="{self}">
    {/if}
    <ul>
        {#each showing as node}
            <svelte:component this="{node}" preview/>
        {/each}
    </ul>
</main>

<style>
    main {
        display: flex;
        flex-direction: column;
        align-items: flex-start;
        padding: 10px;
        gap: 10px;

        width: fit-content;
        min-width: 340px;
        height: fit-content;
        min-height: 48px;

        background: #171717;
        border-radius: 8px;

        font-size: 12px;
    }

    input[type="text"] {
        width: 100%;
        min-width: 320px;
        height: 48px;

        background: #171717;
        border: 1px solid #343434;
        border-radius: 6px;

        padding-left: 10px;
    }

    input[type="text"]:hover {
        border: 1px solid #565656;
    }

    input[type="text"]::placeholder {
        color: #343434;
    }

    ul {
        height: fit-content;
        max-height: 400px;
        width: fit-content;
        overflow-y: scroll;
    }

    ::-webkit-scrollbar {
        width: 6px;
    }

    ::-webkit-scrollbar-track {
        background: transparent;
    }

    ::-webkit-scrollbar-thumb {
        background: #343434;
        cursor: pointer;
    }

    ::-webkit-scrollbar-thumb:hover {
        background: #555;
    }
</style>