<script>
    import TodoList from "./lib/TodoList.svelte";
    import { v4 as uuid } from "uuid";

    let showList = true;

    let todos = null;

    function loadTodos() {
        return fetch('https://jsonplaceholder.typicode.com/todos')
            .then(response => {
                if (response.ok) {
                    return response.json();
                } else {
                    throw new Error("Something went wrong")
                }
            })
    }

    function handleAddTodo (e) {
        const newTitle = e?.detail?.title?.trim();

        if (newTitle) {
            todos = [
                ...todos,
                {
                    id: uuid(),
                    title: e.detail.title,
                    completed: false
                }
            ]
        } else {
            alert("Please enter a valid todo title");
        }
    }

    function handleRemoveTodo (e) {
        const id = e?.detail?.id;

        if (id) {
            todos = todos.filter(todo => todo.id !== id)
        }
    }

    function handleToggleTodo (e) {
        const { id, value } = e?.detail;

        if (id) {
            todos = todos.map(todo => {
                if (todo.id === id) {
                    return {
                        ...todo,
                        completed: value
                    }
                }
                else {
                    return todo;
                }
            })
        }
    }
</script>

<label>
    <input type="checkbox" bind:checked={showList}>
    Show/hide list
</label>

{#if showList}
    {#await loadTodos() then todos}
        <div style:max-width="400px">
            <TodoList 
                {todos} 
                on:addtodo={handleAddTodo}
                on:removetodo={handleRemoveTodo}
                on:toggletodo={handleToggleTodo}
            />
        </div>
    {/await}
{/if}
<style>

</style>