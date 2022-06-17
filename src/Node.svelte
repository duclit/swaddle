<script>
    export let type;
</script>

<main>
    <slot></slot>
    <type>{type}</type>
</main>

<style>
    main {
        display: flex;
        flex-direction: row;
        align-items: center;
        padding: 10px;
        gap: 10px;

        width: fit-content;
        height: fit-content;
        min-height: 48px;

        background: #171717;
        border-radius: 8px;

        font-size: 12px;
    }
</style>