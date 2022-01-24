<script>
  import RandomPanel from './lib/RandomPanel.svelte'
</script>

<main>
  <RandomPanel
    title={"Ban Phrase"}
    description={"Phase banned from catchup meeting"}
    listName={"catchup-ban"}
  />
  <hr/>
  <RandomPanel
    title={"Team"}
    listName={"catchup-team"}
  />
  <hr/>
  <RandomPanel
    title={"Product Designer"}
    listName={"designer"}
  />
  <hr/>
  <RandomPanel
    title={"Software Engineer"}
    listName={"engineer"}
  />
</main>

<style>
  @import url('https://fonts.googleapis.com/css2?family=Kanit:wght@400;700&display=swap');

  main {
    font-family: 'Kanit', serif;
  }
</style>
