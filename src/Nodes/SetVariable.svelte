<script>
    import Node from '../Node.svelte';
    import Input from '../Input.svelte';

    export let preview = false;
</script>

<Node type="Variable_Assignment">set variable <Input is="name" placeholder="Name" isPreview={preview} onlyText/> to <Input is="value" placeholder="Value" isPreview={preview}/></Node>
