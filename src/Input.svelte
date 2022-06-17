<script>
    export let placeholder = "";
    export let isPreview = false;
    export let onlyText = false;
    export let is;
    export let self;

    function handleKeyup(event) {}
</script>

{#if onlyText}
    <!-- svelte-ignore avoid-is -->
    <input type="text" placeholder={placeholder} disabled={isPreview} is={is} spellcheck="false">
{:else}
    <!-- svelte-ignore avoid-is -->
    <div contenteditable={!isPreview} placeholder={placeholder} on:keyup="{handleKeyup}" bind:this="{self}" is={is} spellcheck="false"></div>
{/if}

<style>
    [contenteditable][placeholder]:empty:before {
        content: attr(placeholder);
        color: gray;
    }

    div[contenteditable=true] {
        width: fit-content;
        min-width: 100px;
        height: fit-content;
        min-height: 35px;

        display: flex;
        flex-direction: row;
        align-items: center;

        padding-left: 8px;
        padding-right: 8px;

        background: #171717;
        border: 1px solid #343434;
        border-radius: 4px;

        font-family: 'Source Code Pro';
        font-style: normal;
        font-weight: 400;
        font-size: 12px;
        line-height: 15px;

        color: #FFFFFF;
        overflow: hidden;
    }

    div[contenteditable=false] {
        display: flex;
        flex-direction: row;
        align-items: center;

        padding: 8px;

        width: fit-content;
        height: fit-content;
        min-height: 35px;

        background: #171717;
        border: 1px solid #343434;
        border-radius: 4px;

        color: white;
    }

    div[contenteditable=true]:hover {
        border: 1px solid #565656;
    }

    input[type="text"] {
        width: fit-content;
        min-width: 100px;
        height: 35px;

        padding-left: 8px;
        padding-right: 8px;

        background: #171717;
        border: 1px solid #343434;
        border-radius: 4px;

        font-family: 'Source Code Pro';
        font-style: normal;
        font-weight: 400;
        font-size: 12px;
        line-height: 15px;

        color: #FFFFFF;
    }

    input[type="text"]:disabled {
        background: #171717;
        border: 1px solid #343434;
        border-radius: 4px;
    }

    input[type="text"]:hover, input[type="text"]:is(:disabled) {
        border: 1px solid #565656;
    }
</style>