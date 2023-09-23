<script>
	 export let triviaId;
	 export let winners;
</script>

<div class="body">
	<h1>Trivia {triviaId} winners</h1>
	<div class="row current-players">
		<table style="width: 100%;" >
			<thead>
				<tr>
					<th>Place</th>
					<th>Username</th>
					<th>Score</th>
				</tr>
			</thead>
			<tbody>
				{#each winners as winner, index}
					<tr>
						<td>{index + 1}</td>
						<td>{winner.username}</td>
						<td>{winner.streak}</td>
					</tr>
				{/each}
			</tbody>
		</table>
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
