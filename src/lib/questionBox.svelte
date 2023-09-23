<script>
  export let questionId;
  export let timeLeft;
</script>

<div class="container">
  <div class="first-row">
    <div class="title-element">
      <h2>Question {questionId} </h2>
    </div>
    <div class="title-element">
      <h2>Time left: {timeLeft} </h2>
    </div>
  
  </div>
  <slot/>
</div>

<style>
  .container {
    border-radius: 40px;
    border: 3px darkgray solid;
    padding: 5px;
  }
  .first-row {
    display: flex;
    flex-direction: row;
    justify-content: space-between;
  }

  .title-element {
    text-align: center;
    background-color: bisque;
    padding: 2px 20px 5px 20px;
    border-radius: 30px;
  }
  
</style>

