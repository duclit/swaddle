<script>
    export let items;

    let lastItem = items.at(-1);
    items.pop();
</script>

<main>
    {#each items as item} 
        <p>{item}</p>
        <svg viewBox="0 0 15 15" fill="none" xmlns="http://www.w3.org/2000/svg" width="15" height="15"><path d="M8.146 9.146l-.353.354.707.707.354-.353-.708-.708zM10.5 7.5l.354.354.353-.354-.353-.354-.354.354zM8.854 5.146L8.5 4.793l-.707.707.353.354.708-.708zm0 4.708l2-2-.708-.708-2 2 .708.708zm2-2.708l-2-2-.708.708 2 2 .708-.708zM10.5 7H4v1h6.5V7z" fill="grey"></path></svg>
    {/each}

    <p class="bold">{lastItem}</p>
</main>

<style>
    main {
        width: fit-content;
        height: 16px;

        display: flex;
        flex-direction: row;
        align-items: center;
        gap: 8px;
    }

    p {
        font-family: 'Source Code Pro';
        font-size: 12px;
        line-height: 16px;
        color: #999CA0;
    }

    .bold {
        font-weight: 600;
        color: white;
    }
</style>