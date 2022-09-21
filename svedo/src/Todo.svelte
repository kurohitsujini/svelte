<script>
  import { query, mutation } from "svelte-apollo";
  import { gql } from "apollo-boost";

  const getPersonas = gql`
    {
      persona {
        id
        fullname
        points
        position
        active
        empleados {
          id
          name
          department
        }
      }
    }
  `;
  const todo = query(getPersonas);
  let fullname = "";
  let points, position, empleados_id = 0;

  const addPersona = gql`
    mutation (
      $fullname: String!
      $points: Int!
      $position: Int!
      $empleados_id: Int!
    ) {
      createPersona(
        fullname: $fullname
        points: $points
        position: $position
        empleados_id: $empleados_id
      ) {
        id
        fullname
        points
        position
        active
        empleados {
          id
          name
          department
        }
      }
    }
  `;
  const addMutation = mutation(addPersona);
  async function addTodo() {
    try {
      await addMutation({
        variables: {
          fullname: fullname,
          points: parseInt(points, 10),
          position: parseInt(position, 10),
          empleados_id: parseInt(empleados_id, 10),
        },
        
      });
      todo.refetch();
    } catch (error) {
      console.log("No se puede agregar datos!");
    }
  }
</script>

<div class="col-sm-12 align-items-center">
  <h2>Personas</h2>
  <form on:submit|preventDefault={addTodo}>
    <input type="text" bind:value={fullname} placeholder="fullname" />
    <input type="text" bind:value={points} placeholder="points" />
    <input type="text" bind:value={position} placeholder="position" />
    <input type="text" bind:value={empleados_id} placeholder="Tipo Empleado" />
    <button type="submit">Add</button>
  </form>
  {#if $todo.loading}
    Loading...
  {:else if $todo.error}
    Error: {$todo.error.message}
  {:else}
    <div class="row">
      {#each $todo.data.persona as personas}
        <div class="col-sm-3 m-2">
          <div class="card">
            <img src="..." class="card-img-top" alt="..." />
            <div class="card-body">
              <h5 class="card-title">{personas.fullname}</h5>
              <p class="card-text">{personas.empleados[0].name}</p>
            </div>
            <div class="card-footer text-muted">
              <small class="text-muted">{personas.points}</small>
              <small class="text-muted">{personas.position}</small>
            </div>
          </div>
        </div>
      {/each}
    </div>
  {/if}
</div>
