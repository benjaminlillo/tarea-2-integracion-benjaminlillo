<script>
  export let socket;
  export let sendJoinEvent;
  export let disabled;
  let usernameInput = 'benjalso99';
  console.log("websocket in login component:", socket);
</script>

<div class="body">
	<h1>Dungeons & Trivia</h1>
	<div class="row">
		<div class="username-input-container">
			<input class="username-input" type="text" placeholder="Type your username to start" bind:value={usernameInput}>
		</div>
		<div class="play-button-container">
			<button on:click={() => {sendJoinEvent(usernameInput)}} class="play-button" name="play" {disabled}>{disabled? ('Waiting for websocket connection...') : ('Play')}</button>
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

	.username-input-container {
		background: rgba(255, 255, 255, 0.5);
		width: 300px;
		height: 70px;
		border-radius: 40px;
	}
	
	.username-input {
		border: none;
		background: transparent;
		width: 100%;
		height: 100%;
		border-radius: 40px;
	}


	.play-button-container {
		background: rgba(255, 255, 255, 0.5);
		width: 300px;
		height: 70px;
		border-radius: 40px;
	}

	.play-button {
		background: transparent;
		width: 100%;
		height: 100%;
		border-radius: 40px;
	}
</style>
