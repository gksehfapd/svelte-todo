<script>

    export let todoValue;
    export let handleTodoInputKeyup
</script>

<header>
    <div class="wrap">
        <h1>한동 TODO</h1>
        <input type ="text"
        bind:value={todoValue}
        on:keyup={handleTodoInputKeyup}/>
    </div>
</header>