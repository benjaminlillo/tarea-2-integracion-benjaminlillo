<script>
	import { onMount, onDestroy } from 'svelte';
	import Lobby from '../lib/lobby.svelte';
	import Login from "../lib/login.svelte";
	import Game from '../lib/game.svelte';
	import Highscore from '../lib/highscore.svelte';

	let socket;
	let showGame = false;
	let showLobby = false;
	let showHighscore = false;
	let disablePlayButton = true;
	let winners = [];
	let triviaId = null;
	let joined = null;

	let lobbyMessage = '';
	let lobbySecondsRemaining = 0;
	let lobbyPlayers = [];

	let initQuestion = null;

	onMount(() => {
		if (socket && socket.readyState === WebSocket.OPEN) {
			socket.close();
		}
		socket = new WebSocket('wss://trivia.tallerdeintegracion.cl/connect');

		socket.addEventListener('open', (event) => {
      console.log('WebSocket connection established:', event);
			disablePlayButton = false;
    });
		socket.addEventListener('error', (event) => {
			console.error('WebSocket error:', event);
		});
	});


  let sendJoinEvent = (username) => {
    socket.send(JSON.stringify({"type": "join","id": "18643019","username": username }));
		// recieve answer from server
		socket.addEventListener('message', (event) => {
			const receivedMessage = JSON.parse(event.data);
			console.log('Received message from server in page:', receivedMessage);
			if (receivedMessage.type == "accepted") {
				triviaId = receivedMessage.trivia_id;
				console.log("accepted+++++++", "trivia id:", triviaId, event.data);
				joined = receivedMessage.joined;
				showLobby = true;
			} else if (receivedMessage.type == "denied") {
				console.log("denied", "reason:", receivedMessage.reason);
			} else if (receivedMessage.type == "lobby") {
				console.log("lobby", receivedMessage)
				showGame = false;
				showHighscore = false;
				lobbyMessage = receivedMessage.message;
				lobbySecondsRemaining = receivedMessage.seconds_remaining;
				lobbyPlayers = receivedMessage.players;
				console.log("lobby", lobbyMessage, lobbySecondsRemaining, lobbyPlayers)
			} else if (receivedMessage.type == "question") {
				initQuestion = receivedMessage;
				showLobby = false;
				showGame = true;
			} else if (receivedMessage.type == "highscore") {
				showGame = false;
				showLobby = false;
				showHighscore = true;
				winners = receivedMessage.winners;
			}
		});
  }

	onDestroy(() => {
		if (socket && socket.readyState === WebSocket.OPEN) {
			socket.close();
		}
	});

</script>

<div>
	{#if (!showGame && !showLobby && !showHighscore) }
		<Login bind:socket bind:sendJoinEvent bind:disabled={disablePlayButton}></Login>
	{/if}
	{#if (showGame) }
		<Game bind:socket bind:triviaId bind:joined bind:initQuestion initialQuestion={initQuestion}></Game>
	{/if}
	{#if (showLobby)}
		<Lobby bind:lobbyMessage bind:lobbyPlayers></Lobby>
	{/if}
	{#if (showHighscore)}
		<Highscore triviaId={triviaId} winners={winners}></Highscore>
	{/if}
</div>

<style>

</style>
