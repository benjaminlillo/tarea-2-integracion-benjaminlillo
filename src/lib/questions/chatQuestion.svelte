<script>
  export let socket;
  export let questionTitle;
  export let questionPoints;
  export let resultCorrect;
  export let questionId;
  export let chatMessages;
  export let answer = '';
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
  }
</script>

<div class="container">
  <p>Points: {questionPoints}</p>
  <h2>{questionTitle}</h2>
  <div class="chat-container">
    {#each chatMessages as line}
      <p>{line}</p>
    {/each}
  </div>
  <input id="input" type="text" placeholder="Type your answer" bind:this={input} bind:value={answer} {disabled}>
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

  .chat-container {
    border-radius: 40px;
    border: 3px darkgray solid;
    padding: 5px;
    height: 200px;
    overflow-y: scroll;
    background-color: white;
  }
  
</style>

