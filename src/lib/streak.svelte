<script>
  export let streakName;
  export let streakNumber;
</script>

<div class="container">
  {#if streakNumber == 0}
    <h3>There is no current streak</h3>
  {:else}
    <h3>{streakName} is on a <strong>{streakNumber} question</strong> streak!</h3>
  {/if}
</div>

<style>
  .container {
    background-color: bisque;
    border-radius: 5px;
    padding: 20px;
  }
</style>