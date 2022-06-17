<script>
    import Node from '../Node.svelte';
    import Input from '../Input.svelte';

    export let preview = false;
</script>

<Node type="Function_Call">call function <Input is="name" placeholder="Function name" isPreview={preview} onlyText/> with <Input is="arguments" placeholder="Arguments" isPreview={preview}/></Node>
