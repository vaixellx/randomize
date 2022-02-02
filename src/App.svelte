<script>
import { onMount } from 'svelte';

  import RandomPanel from './lib/RandomPanel.svelte'

  let panels = JSON.parse(localStorage.getItem('panels')) || []
  let adding = false
  let rememberedCollapse = panels.map(panel => { return panel.collapse })

  function addPanel() {
    adding = true
    panels = [
      { name: "XXXXX (Click to edit)", listName: new Date().getTime(), collapse: false },
      ...panels
    ]
    setTimeout(() => { adding = false }, 300)
  }

  function removePanel(removingListName) {
    if (confirm('Are you sure?')) {
      panels = panels.filter(({_name, listName}) => { return listName != removingListName})
    }
  }

  $: localStorage.setItem('panels', JSON.stringify(panels))
</script>

<main>
  <div class="container mt-3">
    <div class="row">
      <div class="col-12 col-sm-6 text-center text-sm-start">
        <h1>Lucky Draw!</h1>
      </div>

      <div class="col-12 col-sm-6 text-center text-sm-end">
        <button type="button" class="mt-lg-1 mt-xl-2 btn btn-primary {adding ? 'disabled' : ''}" on:click="{addPanel}">
          + Add New Panel
        </button>
      </div>

      <h3 class="mt-5 text-muted text-center">
        {#if (panels.length == 0)}
          Click on "+ Add New Panel" to start using App.
        {/if}
      </h3>

      {#each panels as {name, listName, collapse}, i }
        <div class="card mb-3">
          <div class="row mt-2 card-actions">
            <div class="col-1 text-start">
              <a on:click="{e => collapse = !collapse}" href="#collapse-{listName}" aria-expanded="true" class="text-dark" data-bs-toggle="collapse">▾</a>
            </div>

            <div class="col-10">
              <input type="text" class="panel-name border-0 d-block text-center w-100" bind:value="{name}" />
            </div>

            <div class="col-1 text-end">
              <a href="#" on:click="{removePanel(listName)}" class="text-danger">✕</a>
            </div>
          </div>

          <div class="card-body pt-0">
            <div id="collapse-{listName}" class="collapse {rememberedCollapse[i] ? '' : 'show'}">
              <RandomPanel listName={listName} />
            </div>
          </div>
        </div>
      {/each}
    </div>
</main>

<style>
  @import url('https://fonts.googleapis.com/css2?family=Kanit:wght@400;700&display=swap');

  main {
    font-family: 'Kanit', serif;
  }

  .panel-name {
    font-size: 2rem;
  }

  .card-actions a {
    text-decoration: none;
  }
</style>
