<script>
  let personajes = [];
  let cargando = true;
  let error = "";

  async function obtenerPersonajes() {
    try {
      const respuesta = await fetch(
        "https://rickandmortyapi.com/api/character",
      );

      if (!respuesta.ok) {
        throw new Error("No se pudieron cargar los personajes");
      }

      const datos = await respuesta.json();
      personajes = datos.results;
    } catch (e) {
      error = e instanceof Error ? e.message : "Error desconocido";
    } finally {
      cargando = false;
    }
  }

  obtenerPersonajes();
</script>

<h1>Personajes de Rick and Morty</h1>

{#if cargando}
  <p>Cargando personajes...</p>
{:else if error}
  <p>{error}</p>
{:else}
  <section class="contenedor">
    {#each personajes as personaje (personaje.id)}
      <article class="tarjeta">
        <img src={personaje.image} alt={personaje.name} />
        <h2>{personaje.name}</h2>
        <p>Estado: {personaje.status}</p>
        <p>Especie: {personaje.species}</p>
      </article>
    {/each}
  </section>
{/if}

<style>
  h1 {
    text-align: center;
  }

  .contenedor {
    display: flex;
    flex-wrap: wrap;
    justify-content: center;
    gap: 20px;
  }

  .tarjeta {
    width: 220px;
    padding: 15px;
    border: 1px solid #ccc;
    border-radius: 10px;
    text-align: center;
  }

  .tarjeta img {
    width: 100%;
    border-radius: 8px;
  }
</style>
