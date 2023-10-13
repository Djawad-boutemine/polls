<script>
import PollStore from "../stores/PollStore";
import Button from "../shared/Button.svelte";
import {
  createEventDispatcher
} from "svelte";
  import PollDetails from "./pollDetails.svelte";
let dispathch = createEventDispatcher();
let fields = {
  question: '',
  answerA: '',
  answerB: ''
};
let error = {
  question: '',
  answerA: '',
  answerB: ''
};
let valid = false;
const submitHundler = () => {
  valid = true;
  // validate question
  if (fields.question.trim().length < 5) {
    valid = false;
    error.question = 'Question must at least 5 characters long';
  } else {
    error.question = '';
  }
  // validate answer a
  if (fields.answerA.trim().length < 1) {
    valid = false;
    error.answerA = 'Answer A cannot be empty';
  } else {
    error.answerA = '';
  }
  // validate  answer b
  if (fields.answerB.trim().length < 1) {
    valid = false;
    error.answerB = 'Answer B cannot be empty';
  } else {
    error.answerB = '';;
  }
  // Adding a new poll
  if (valid) {
    let poll = {
      ...fields,
      voteA: 0,
      voteB: 0,
      id: Math.random(),
    }
    PollStore.update(currentPolls => {
      return [poll,...currentPolls]
    } )
    fields.question = '';
    fields.answerA = '';
    fields.answerB = '';
    dispathch('add');
  }
}
</script>

<form on:click|preventDefault={submitHundler} on:keydown>
    <div class='form-field'>
        <label for='question'>Poll Question:</label>
        <input type="text" id='questoin' bind:value={fields.question}>
        <div class="error">{error.question}</div>
    </div>
    <div class='form-field'>
        <label for='answer-a'>Answer A:</label>
        <input type="text" id='answer-a' bind:value={fields.answerA}>
        <div class="error">{error.answerA}</div>
    </div>
    <div class='form-field'>
        <label for='answer-b'>Answer B:</label>
        <input type="text" id='answer-b' bind:value={fields.answerB}>
        <div class="error">{error.answerB}</div>
    </div>
    <Button type='secondary' flat=true >Add Poll</Button>
</form>

<style>
form {
  width: 400px;
  margin: 0 auto;
  text-align: center;
}

.form-field {
  margin: 18px auto;
}

input {
  width: 100%;
  border-radius: 6px;
}

label {
  margin: 10px auto;
  text-align: left;
}

.error {
  font-weight: bold;
  font-size: 12px;
  color: #d91b42;
}
</style>
