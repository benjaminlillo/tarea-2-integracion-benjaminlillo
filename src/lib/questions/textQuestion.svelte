<script>
  /** @type {WebSocket}*/
  export let socket;
  export let questionTitle;
  export let questionPoints;
  export let resultCorrect;
  export let answer = '';
  export let questionId;
  export let disabled;

  let input;
  let button;

  const handleKeyPress = (event) => {
    if (event.key === 'Enter') {
      button.click();
    }
  };

  $: {
    if (input && button) {
      input.addEventListener('keypress', handleKeyPress);
    }
  }


  const sendAnswer = () => {
    socket.send(JSON.stringify({
      type: "answer",
      question_id: questionId,
      value: answer,
    }));
    disabled = true;
  }

</script>

<div class="container">
  <h2>{questionTitle}</h2>
  <p>Points: {questionPoints}</p>
  <input id="input" type="text" placeholder="Type your answer" {disabled} bind:this={input} bind:value={answer}>
  <button id="button" type="submit" bind:this={button} on:click={sendAnswer} {disabled}>send</button>
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

