<script>
	import { onMount, onDestroy } from 'svelte';
  import QuestionBox from "$lib/questionBox.svelte"
	import ScoreBoard from "$lib/scoreBoard.svelte";
	import TextQuestion from "$lib/questions/textQuestion.svelte";

	let initQN = 5;
	let initTL = 10;
	onMount(() => {
		// get the WebSocket object from the query parameter
		const urlParams = new URLSearchParams(window.location.search);
		const wsString = urlParams.get('ws');
		const ws = JSON.parse(wsString, (key, value) => {
			if (key === 'onmessage' || key === 'onopen' || key === 'onclose' || key === 'onerror') {
				return eval('(' + value + ')');
			}
			return value;
		});
		console.log("websocket in game:", ws);
	});
</script>

<div class="navbar">
	<h1>Dungeons & Trivia</h1>
	<div class="notifications">
		hola
	</div>
</div>
<div class="row">
  <div class="question-box">
    <QuestionBox questionNumber={initQN} timeLeft={initTL}></QuestionBox>
  </div>
  <div class="score-board">
    <ScoreBoard></ScoreBoard>
  </div>
</div>

<style>
  .row {
		width: 100%;
		display: flex;
		flex-direction: row;
		align-items: stretch;
	}

	.navbar {
		width: 100%;
		display: flex;
		flex-direction: row;
		justify-content: space-between;
	}

	.question-box {
		flex-grow: 2;
	}

	.score-board {
		flex-grow: 1;
	}
</style>
