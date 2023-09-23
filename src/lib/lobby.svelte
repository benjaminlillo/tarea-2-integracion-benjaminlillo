<script>
	 export let lobbyMessage;
	 export let lobbyPlayers;
</script>

<div class="body">
	<h1>Lobby</h1>
	{lobbyMessage? (lobbyMessage) : ('Waiting for server...')}
	<div class="row">
		<div class="current-players">
			<h3>Current players</h3>
			<br>
			{#each lobbyPlayers as player}
			{player + ' '} 
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
