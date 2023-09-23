<script>
  export let socket;
  export let questionTitle;
  export let questionPoints;
  export let resultCorrect;
  export let questionOptions;
  export let questionId;
  export let disabled;

  const sendAnswer = (answer) => {
    socket.send(JSON.stringify({
      type: "answer",
      question_id: questionId,
      value: answer,
    }));
    disabled = true;
  }
</script>

<div class="container">
  <p>Points: {questionPoints}</p>
  <h2>{questionTitle}</h2>
  {#each Object.keys(questionOptions) as number}
    <button on:click={() => sendAnswer(number)} {disabled}>{questionOptions[number]}</button>
  {/each}
  {#if resultCorrect != null}
    <h1>Result: {resultCorrect? ("CORRECT") : ("INCORRECT")}</h1>
  {/if}
</div>

<style>
  .container {
    border-radius: 40px;
    border: 3px darkgray solid;
    padding: 5px;
  }
  
</style>

