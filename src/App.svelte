<script>
import Header from "./components/Header.svelte";
import Footer from "./components/Footer.svelte";
import Tabs from "./shared/Tabs.svelte";
import CreatPollForm from "./components/CreatPollForm.svelte";
import PollList from "./components/PollList.svelte";
// tabs
let items = ['Current Polls', 'Add New Poll'];
let activeItem = 'Current Polls';
const tabChange = (e) => {
  activeItem = e.detail;
}

const handleAdd = (e) => {
  activeItem = 'Current Polls' ;
}
import {tweened} from 'svelte/motion' ;
const value = tweened(0);
</script>
<Header/>
    <main>
        <Tabs activeItem={activeItem} items={items} on:tabChange={tabChange} />
        {#if activeItem === 'Current Polls'}
        <PollList/>
            {:else if activeItem === 'Add New Poll'}
            <CreatPollForm on:add={handleAdd}> </CreatPollForm>
            {/if}
            </main>
            <Footer/>

<style>
main {
  max-width: 960px;
  margin: 40px auto;
}
</style>
