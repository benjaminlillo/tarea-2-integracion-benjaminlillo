<script>
  import QuestionBox from "$lib/QuestionBox.svelte";
  import ScoreBoard from "$lib/ScoreBoard.svelte";
  import Streak from "./streak.svelte";
  import TextQuestion from "$lib/questions/textQuestion.svelte";
  import ButtonQuestion from "./questions/buttonQuestion.svelte";
  import ChatQuestion from "./questions/chatQuestion.svelte";
	export let socket;
  export let triviaId;
  export let initialQuestion;
  let disabled = false;
  // timer event
  let timeLeft = 0;
  // question event
  let questionId = null;
  let questionType = null;
  let questionTitle = null;
  let questionPoints = [];
  let questionOptions = null;
  if (initialQuestion) {
    questionId = initialQuestion.question_id;
    questionType = initialQuestion.question_type;
    questionTitle = initialQuestion.question_title;
    questionPoints = initialQuestion.question_points;
    questionOptions = initialQuestion.question_options;
  }
  // streak event
  let streakName = "";
  let streakNumber = 0;
  // result event
  let resultCorrect = null;
  // score event
  let scores = {};
  let orderedScoreNames = [];
  // chat event
  let chatMessages = [];
  socket.addEventListener('message', (event) => {
    let data = JSON.parse(event.data);
    if (data.type == "question") {
      questionId = data.question_id;
      questionType = data.question_type;
      questionTitle = data.question_title;
      questionPoints = data.question_points;
      questionOptions = data.question_options;
      resultCorrect = null;
      disabled = false;
      chatMessages = [];
    } else if (data.type == "streak") {
      streakName = data.username;
      streakNumber = data.streak;
    } else if (data.type == "result") {
      if (data.question_id == questionId){
        resultCorrect = data.correct;
        console.log("result correct:", resultCorrect);
        if (resultCorrect) {
          disabled = true;
        }
      }
    } else if (data.type == "timer") {
      timeLeft = data.seconds_remaining;
    } else if (data.type == "score") {
      scores = data.scores;
      orderedScoreNames = Object.keys(scores).sort((a, b) => scores[b] - scores[a]);
      console.log("ordered score names:", orderedScoreNames);
    } else if (data.type == "chat") {
      let date = new Date();
      let hours = date.getHours();
      let minutes = date.getMinutes();
      let seconds = date.getSeconds();
      let time = `${hours}:${minutes}:${seconds}`;
      chatMessages.push(`${time} - ${data.username}: ${data.message}`);
      chatMessages = chatMessages;
      console.log("chat messages: new +++++++++", chatMessages);
    } else if (data.type == "disconnected") {
      console.log("disconnected", data.message);
    }
  });
</script>

<div class="navbar">
  <div>
    <h1>Dungeons & Trivia</h1>
    <h2>Trivia {triviaId}</h2>
  </div>
  <Streak bind:streakName bind:streakNumber></Streak>
</div>
<div class="row">
  <div class="question-box">
    <QuestionBox bind:questionId bind:timeLeft>
      {#if (questionType == "text")}
        <TextQuestion bind:socket bind:resultCorrect bind:disabled questionId={questionId} questionTitle={questionTitle} questionPoints={questionPoints}></TextQuestion>
      {:else if (questionType == "button")}
        <ButtonQuestion bind:socket bind:resultCorrect bind:disabled questionId={questionId} questionTitle={questionTitle} questionPoints={questionPoints} questionOptions={questionOptions}></ButtonQuestion>
      {:else if (questionType == "chat")}
        <ChatQuestion bind:socket bind:resultCorrect bind:chatMessages bind:disabled questionId={questionId} questionTitle={questionTitle} questionPoints={questionPoints}></ChatQuestion>
      {/if}
    </QuestionBox>
  </div>
  <div class="score-board">
    <ScoreBoard bind:scores bind:orderedScoreNames></ScoreBoard>
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
