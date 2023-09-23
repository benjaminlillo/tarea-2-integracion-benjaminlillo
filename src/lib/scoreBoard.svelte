<script>
  export let scores;
  export let orderedScoreNames;
</script>

<div class="container">
  <h1>Scoreboard</h1>
  <table style="width: 100%;" >
    <thead>
      <tr>
        <th>Place</th>
        <th>Username</th>
        <th>Score</th>
      </tr>
    </thead>
    <tbody>
      {#each orderedScoreNames as username, index}
        <tr>
          <td>{index + 1}</td>
          <td>{username}</td>
          <td>{scores[username]}</td>
        </tr>
      {/each}
    
    </tbody>
  </table>
</div>

<style>
  .container {
    border-radius: 40px;
    background-color: bisque;
  }
  
</style>

