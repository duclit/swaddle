<script>
    import Node from '../Node.svelte';
    import Input from '../Input.svelte';

    export let preview = false;
</script>

<Node type="Print">print <Input is="value" placeholder="Expression" isPreview={preview}/></Node>
