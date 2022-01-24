<script>
  export let title = 'Random'
  export let description
  export let listName = title.replace(' ', '_').toLowerCase()

  let rawCandidates = localStorage.getItem(listName)
  let result = null
  let randomInterval = null
  let wonCandidates = []
  let candidatesToRandom = []

  $: candidates = rawCandidates?.split("\n")?.filter((candidate) => {
    return candidate.length > 0
  }) || []
  $: randoming = randomInterval != null
  $: randomable = wonCandidates.length != candidates.length

  function random() {
    if (wonCandidates.length == 0) {
      candidatesToRandom = [...candidates]
    }

    randomInterval = setInterval(() => {
      result = candidatesToRandom[Math.floor(Math.random() * candidatesToRandom.length)]
    }, 200)

    setTimeout(stopRandom, 4000)
  }

  function stopRandom() {
    clearInterval(randomInterval)
    randomInterval = null
    wonCandidates = [...wonCandidates, result]
    candidatesToRandom.splice(candidatesToRandom.indexOf(result), 1)
  }

  function onCandidatesChanged(event) {
    const value = event.target.value
    stopRandom()
    wonCandidates = []
    localStorage.setItem(listName, value)
  }

</script>

<div class="container">
  <h2 class="mt-3 text-center" data-bs-toggle="collapse" href="#random-panel-{listName}">{title}</h2>
  {#if description }
    <div class="text-center text-muted">{description}</div>
  {/if}

  <div class="collapse" id="random-panel-{listName}">
    <div class="text-center mt-4 mb-3">
      <a class="btn btn-lg btn-primary px-5 py-2 {randomable ? "" : "disabled"}" on:click={random}>
        Random !!
      </a>
    </div>

    <div class="row">
      <div class="col"></div>
      <div class="col-12 col-lg-6">
        <div class="card border-0 random-result {randoming ? 'randoming' : ''} {result && !randoming ? 'done' : ''}">
          <div class="card-body">
            { result || '????' }
          </div>
        </div>
      </div>
      <div class="col"></div>
    </div>

    <div class="mt-3 text-center">
      <h6><strong>Random Items</strong></h6>
      {#if candidates.length == 0}
        <div class="text-muted">No items to random, please click on "Manage Random Items" to add some.</div>
      {/if}
      {#each candidates as candidate, i }
        {#if wonCandidates.indexOf(candidate) != -1}
          <del class="mx-2">{candidate} ({wonCandidates.indexOf(candidate) + 1})</del>
        {:else}
          <span class="mx-2 {candidate === result ? 'text-info' : ''}">{candidate}</span>
        {/if}
      {/each}

      <div class="mt-3 mx-auto" style="max-width: 480px">
        <a class="btn btn-sm btn-link mb-2" data-bs-toggle="collapse" href="#random-item-{listName}">Manage Random Items</a>
        <div id="random-item-{listName}" class="collapse">
          <textarea name="random-item-{listName}"
            class="form-control"
            bind:value="{rawCandidates}"
            on:keyup="{onCandidatesChanged}"
            rows="10"></textarea>
          <small class="text-muted">One items per line</small>
        </div>
      </div>
    </div>
  </div>
</div>

<style>
  .random-result {
    background-color: #003;
    color: #aaa;
    font-size: 2rem;
    line-height: 4rem;
    text-align: center;
    transition: color .3s, backgorund-color .3s, font-size .3s;
  }

  .random-result.randoming {
    animation-name: flicker;
    animation-iteration-count: infinite;
    animation-duration: .6s;
  }

  .random-result.done {
    animation-name: wavy;
    animation-iteration-count: infinite;
    animation-duration: 2s;
    color: #47bfaf;
    font-size: 3rem;
    font-weight: 700;
  }

  del {
    color: #aaa;

  }

  @keyframes wavy {
    0% { font-size: 2.5rem; }
    25% { font-size: 3rem; }
    50% { font-size: 2rem; }
    75% { font-size: 4rem; }
    100% { font-size: 2.5rem; }
  }

  @keyframes flicker {
    0% {
      background-color: #000;
      color: #aaa;
    }

    50% {
      background-color: #111;
      color: #333;
    }

    100% {
      background-color: #000;
      color: #aaa;
    }
  }
</style>
