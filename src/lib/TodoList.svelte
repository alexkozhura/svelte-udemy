<script>
    import { createEventDispatcher, afterUpdate } from "svelte";  
    import Button from "./Button.svelte";
    import FaRegTrashAlt from "svelte-icons/fa/FaRegTrashAlt.svelte";
    
    export let todos = [];
    let todoListElement, autoScroll, listDivScrollHeight;
    let inputText = "";
    let prevTodos = todos;
    const dispatch = createEventDispatcher();


    afterUpdate(() => {
        if (autoScroll) {
                todoListElement.scrollTo(0, listDivScrollHeight);
        }
        autoScroll = false;
    })

    $: {
        autoScroll = todos.length > prevTodos.length;
        prevTodos = todos;
    }


    function handleAddTodo() {
        dispatch("addtodo", {
            title: inputText
        })
        inputText = "";
    }

    function handleRemoveTodo(id) {
        dispatch("removetodo", {
            id: id
        })
    }
    
    function handleToggleTodo(id, value) {
        dispatch("toggletodo", {
            id,
            value
        })
    }
</script>

<div class="todo-list-wrapper">
    <div 
        class="todo-list" 
        bind:this={todoListElement}
    >
        <div bind:offsetHeight={listDivScrollHeight}>
            {#if todos.length === 0}
                <p class="no-items-text">No todos yet</p>
            {:else}
            <ul>
                {#each todos as { id, title, completed } (id)}
                    <li class:completed> 
                        <label>
                            <input 
                                on:input={(e) => {
                                    e.currentTarget.checked = completed;
                                    handleToggleTodo(id, !completed)
                                }} 
                                type="checkbox" 
                                checked={completed}
                            />
                            {title}
                        </label>
                        <button 
                            class="remove-todo-button"
                            on:click={() => handleRemoveTodo(id)}
                            aria-label="Remove todo: {title}">
                            <span 
                                style:display="inline-block"
                                style:width="10px"><FaRegTrashAlt />
                            </span>
                        </button>
                    </li>
                {/each}
            </ul>
            {/if}
        </div>
    </div>
    <form class="add-todo-form" on:submit|preventDefault={handleAddTodo}>
        <input bind:value={inputText} placeholder="New Todo"/>
        <Button 
            class="add-todo-button" 
            type="submit" 
            disabled={!inputText}
        >Add</Button>
    </form>
</div>

<style lang="scss">
    .todo-list-wrapper {
        background-color: #424242;
        border: 1px solid #4b4b4b;
        .no-items-text {
            margin: 0;
            padding: 15px;
            text-align: center;
        }
        .todo-list {
        max-height: 200px;
        overflow: auto;
        ul {
            margin: 0;
            padding: 10px;
            list-style: none;
            li {
                margin-bottom: 5px;
                display: flex;
                align-items: center;
                background-color: #303030;
                border-radius: 5px;
                padding: 10px;
                position: relative;
                label {
                    cursor: pointer;
                    font-size: 18px;
                    display: flex;
                    align-items: baseline;
                    padding-right: 20px;
                    input[type="checkbox"] {
                        margin: 0 10px 0 0;
                        cursor: pointer;
                    }
                }
                &.completed > label{
                    opacity: 0.5;
                    text-decoration: line-through;
                }
                .remove-todo-button {
                    border: none;
                    background: none;
                    padding: 5px;
                    position: absolute;
                    right: 10px;
                    cursor: pointer;
                    display: none;
                    :global(svg) {
                        fill: #bd1414;
                    }
                }
                &:hover {
                    .remove-todo-button {
                        display: block;
                    }
                }
            }
        }
        }
        .add-todo-form {
            padding: 15px;
            background-color: #303030;
            display: flex;
            flex-wrap: wrap;
            border-top: 1px solid #4b4b4b;
            input {
                flex: 1;
                background-color: #424242;
                border: 1px solid #4b4b4b;
                padding: 10px;
                color: white;
                border-radius: 5px;
                margin-right: 10px;
            }
    }
}
    
    
</style>