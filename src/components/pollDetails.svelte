<script>
import Button from "../shared/Button.svelte";
import PollStore from "../stores/PollStore";
import Card from "../shared/Card.svelte";
export let poll;
import { tweened } from 'svelte/motion' ;
// reactive values
$: totalVotes = poll.voteA + poll.voteB;
$: percentA = Math.floor(100 / totalVotes * poll.voteA) || 0;
$: percentB = Math.floor(100 / totalVotes * poll.voteB) || 0;
const tweenedA = tweened(0) ;
const tweenedB = tweened(0) ;
$:tweenedA.set(percentA) ;
$:tweenedB.set(percentB) ;
const handleVote = (option, id) => {
  PollStore.update(currentPolls => {
    let copiedPolls = [...currentPolls];
    let upVotedPoll = copiedPolls.find((poll) => poll.id == id);
    if (option === 'a') {
      upVotedPoll.voteA++;
    }
    if (option === 'b') {
      upVotedPoll.voteB++;
    }
    return copiedPolls;
  })
}
const handleDelete = (id) => {
  PollStore.update((currentPolls) => {
    return currentPolls.filter((poll) => {
      return poll.id != id;
    })
  })
}
</script>

<Card>
    <div class="poll">
        <h3>{poll.question}</h3>
        <p>Total votes : {totalVotes}</p>
        <div class="answer" on:click={()=>handleVote('a',poll.id)} on:keydown>
            <div class="percent percent-a" style="width:{$tweenedA}%"></div>
            <span> {poll.answerA} ({poll.voteA})</span>
        </div>
        <div class="answer" on:click={()=>handleVote('b',poll.id)} on:keydown>
            <div class="percent percent-b" style="width:{$tweenedB}%"></div>
            <span> {poll.answerB} ({poll.voteB})</span>
        </div>
        <div class="delete" on:click={()=>handleDelete(poll.id)} on:keydown>
            <Button flat={true} >Delete</Button>
        </div>
    </div>
</Card>

<style>
h3 {
  margin: 0 auto;
  color: #555;
}

p {
  margin-top: 6px;
  font-size: 14px;
  color: #aaa;
  margin-bottom: 30px;
}

.answer {
  background: #fafafa;
  cursor: pointer;
  margin: 10px auto;
  position: relative;
}

.answer:hover {
  opacity: 0.6;
}

span {
  display: inline-block;
  padding: 10px 20px;
}

.percent {
  height: 100%;
  position: absolute;
  box-sizing: border-box;
}

.percent-a {
  background: rgba(217, 27, 66, 0.2);
  border-left: 2px solid #d91b42;
}

.percent-b {
  background: rgba(69, 196, 150, 0.2);
  border-left: 2px solid #45c496;
}

.delete {
  margin-top: 30px;
  text-align: center;
}
</style>
