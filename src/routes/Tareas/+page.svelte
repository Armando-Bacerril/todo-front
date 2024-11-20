<script>
    import { onMount } from 'svelte';

    let tarea = ""
    let lista_de_tareas = []

    onMount(() => {
    const userLoggedIn = localStorage.getItem('sessionToken');
    if (!userLoggedIn) {
        window.location.href = '/login.html';
    }
});

    onMount(() => {
        obtenerDatos()
    })

    async function obtenerDatos(){
        let response = await fetch('/api/obtenerDatos')
        let datos = await response.json()
        lista_de_tareas = datos
    }

    async function agregarTarea(){
        if (tarea.length === 0) {
            alert("Pusiste una tarea vacía")
        } else {
            let response = await fetch('/api/agregarTarea', {
                method: 'POST',
                headers: {
                    'Content-Type': 'application/json'
                },
                body: JSON.stringify({
                    nombre_tarea: tarea,
                    completado: false
                })
            })
        }
        obtenerDatos()
    }

    async function eliminarTarea(id){
        let response = await fetch ('/api/eliminarTarea', {
            method: 'DELETE',
            headers: {
                'Content-Type': 'application/json'
            },
            body: JSON.stringify({
                id: id
            })
        })
        obtenerDatos()
    }

    async function completarTarea(tarea) {
        let response = await fetch ('/api/completarTarea', {
            method: 'POST',
            headers: {
                'Content-Type': 'application/json'
            },
            body: JSON.stringify({
                id: tarea.id,
                completado: tarea.completado
            })
        })
    }

    async function CerrarSesion() {
        localStorage.removeItem('sessionToken');
        window.location.href = '/login.html';  // Redirige al login
    };
</script>

<main>
    <header>
        <nav>
            <ul>
                <li><a href="/Tareas" class="nav-button">Tareas</a></li>
                <li><a href="/Analytics" class="nav-button">Analytics</a></li>
                <li><button on:click={CerrarSesion} class="nav-button">Cerrar Sesión</button></li>
            </ul>
        </nav>
    </header>

    <h1>Lista de tareas</h1>

    <div class="input-container">
        <input type="text" placeholder="Escribe una tarea aquí" bind:value={tarea} />
        <button on:click={agregarTarea}>Agregar</button>
    </div>

    <ul>
        {#each lista_de_tareas as tarea, indice}
        <li>
            <input type="checkbox" bind:checked={tarea.completado} on:change={() => completarTarea(tarea)} />
            <span class:completado={tarea.completado}>{tarea.nombre_tarea}</span>
            <button class="eliminar" on:click={() => eliminarTarea(tarea.id)}>Eliminar</button>
        </li>
        {/each}
    </ul>
</main>

<style>
    main {
        max-width: 600px;
        margin: 0 auto;
        padding: 2rem;
        background-color: white;
        border-radius: 8px;
        box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
    }

    h1 {
        text-align: center;
        font-size: 1.5rem;
        margin-bottom: 1.5rem;
    }

    header {
        background-color: #6200ea;
        color: white;
        padding: 1rem 0;
        text-align: center;
        border-radius: 8px 8px 0 0;
        margin-bottom: 1.5rem;
    }

    nav ul {
        list-style-type: none;
        padding: 0;
        margin: 0;
    }

    nav ul li {
        display: inline-block;
        margin-right: 20px;
    }

    .nav-button {
        color: white;
        text-decoration: none;
        font-size: 1rem;
        padding: 8px 15px;
        border-radius: 5px;
        transition: background-color 0.3s ease;
        background-color: #6200ea;
    }

    .nav-button:hover {
        background-color: #3700b3; 
    }

    button {
        border: none;
        background-color: transparent;
        cursor: pointer;
    }

    .input-container {
        display: flex;
        justify-content: center;
        margin-bottom: 1.5rem;
    }

    input[type="text"] {
        width: calc(100% - 100px);
        padding: 0.5rem;
        margin-right: 0.5rem;
        border-radius: 5px;
        border: 1px solid #ccc;
    }

    button {
        padding: 0.5rem;
        background-color: #6200ea;
        color: white;
        border: none;
        cursor: pointer;
        border-radius: 5px;
        transition: background-color 0.3s ease;
    }

    button:hover {
        background-color: #3700b3;
    }

    ul {
        list-style-type: none;
        padding: 0;
    }

    li {
        display: flex;
        align-items: center;
        justify-content: space-between;
        padding: 1rem;
        background-color: #f9f9f9;
        border-radius: 5px;
        margin-bottom: 1rem;
        box-shadow: 0 1px 2px rgba(0, 0, 0, 0.1);
    }

    li:hover {
        background-color: #efefef;
    }

    .completado {
        text-decoration: line-through;
        color: gray;
    }

    .eliminar {
        background-color: red;
        color: white;
        padding: 0.5rem 1rem;
        border: none;
        border-radius: 5px;
        cursor: pointer;
    }

    .eliminar:hover {
        background-color: darkred;
    }

    input[type="checkbox"] {
        margin-right: 10px;
    }
</style>
