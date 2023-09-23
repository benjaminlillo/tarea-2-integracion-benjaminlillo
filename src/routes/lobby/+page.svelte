<script>
	/**@type {number}*/
	 export let waitingTime = 0;
	 export let currentPlayers = [
    {
      username: "benjalso99",
    },
    {
      username: "benjalso88",
    },
  ];
</script>

<div class="body">
	<h1>Lobby</h1>
	Waiting {waitingTime} seconds for other players to join
	<div class="row">
		<div class="current-players">
			Current players
			<br>
			{#each currentPlayers as player}
			{player.username + ' '} 
			{/each}
		</div>
	</div>
</div>

<style>
	/* :global(body){
    background-image: url($static/shrine_out.jpg);
	} */
	:global(body){
    background-color: bisque;
	}
  .row {
		position: absolute;
		bottom: 30%;
		width: 100%;
		display: flex;
		flex-direction: row;
		justify-content: space-around;
		align-items: stretch;
	}

	.current-players {
		text-align: center;
		background-color: rgba(255, 255, 255, 0.5);
		border-radius: 30px;
		width: 400px;
		padding: 20px;
	}

</style>
