<script>
  import { onMount } from 'svelte';

onMount(() => {
    const userLoggedIn = localStorage.getItem('sessionToken');
    if (!userLoggedIn) {
        window.location.href = '/login.html';
    }
});

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
<h1>
    En proceso...
</h1>
</main>
<style>
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
    background-color: #6200ea; /* Fondo de los botones */
}

.nav-button:hover {
    background-color: #3700b3; /* Color cuando se pasa el mouse */
}

button {
    border: none;
    background-color: transparent;
    cursor: pointer;
}
</style>