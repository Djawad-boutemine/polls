<script>
  import {fade,slide,scale} from 'svelte/transition';
import PollDetails from "./pollDetails.svelte";
import PollStore from '../stores/PollStore.js';
import {flip} from 'svelte/animate'
</script>
<div class="poll-list">
    {#each $PollStore as poll (poll.id)}
    <div in:fade out:scale|local animate:flip={{duration:500}}>
        <PollDetails {poll} ></PollDetails>
    </div>
    {/each}
</div>

<style>
.poll-list {
  display: grid;
  grid-template-columns: 1fr 1fr;
  grid-gap: 25px;
}
</style>
