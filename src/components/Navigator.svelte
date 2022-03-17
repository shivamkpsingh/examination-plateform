<script>
	import { createEventDispatcher } from 'svelte';
	import Confirmation from './Confirmation.svelte';
	import { reviewNavigator, chooseAns } from '../store.js';
	import Sidebar from './Sidebar.svelte';
	import Timer from './Timer.svelte';
	const dispatch = createEventDispatcher();
	export let currentQues; //for changing current question (for next and pre)
	let sidebar_show = false;
	let confirm_show = false;
	export let questionId;
	questionId = Number(questionId);
	// previous page function
	const prevPage = () => {
		dispatch('prevPage');
	};
	// next page function
	const nextPage = () => {
		dispatch('nextPage');
	};
    //  update question click on sidebar
	const displayQuesNum = (event) => {
		dispatch('updateQues', event.detail);
	};
</script>

<div class="navigator">
	<div class="navigator__option d-flex justify-content-between p-2 align-items-center">
		{#if !$reviewNavigator}
			<Timer />
		{/if}
		<!-- svelte-ignore a11y-accesskey -->
		<button on:click={() => (sidebar_show = !sidebar_show)} class="btn btn-light" accesskey="l">
			List
		</button>
		<Sidebar bind:show={sidebar_show} on:displayQuesNum={displayQuesNum} />
		{#if currentQues < 1}
			<button disabled>Previous</button>
		{:else}
			<button class="btn btn-light" on:click={prevPage}>Previous</button>
		{/if}
        {#if  !$reviewNavigator}
			<span>{currentQues + 1} of 11</span>
        {:else}
			<span>{questionId + 1} of 11</span>
        {/if}
		{#if currentQues + 1 > 10}
			<!-- svelte-ignore a11y-accesskey -->
			<button disabled accesskey="n">Next</button>
		{:else}
			<!-- svelte-ignore a11y-accesskey -->
			<button on:click={nextPage} class="btn btn-light" accesskey="n">Next</button>
		{/if}
		<!-- svelte-ignore a11y-accesskey -->
		{#if !$reviewNavigator}
			<button class="btn btn-light" accesskey="t" on:click={() => (confirm_show = !confirm_show)}
				>End Test</button
			>
		{:else}
			<a href="/">
				<!-- svelte-ignore a11y-accesskey -->
				<button accesskey="b" class="btn btn-light">Dashboard</button>
			</a>
		{/if}
	</div>
</div>
<Confirmation bind:show={confirm_show} />
