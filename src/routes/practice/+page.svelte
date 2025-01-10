<script lang="ts">
	import Navbar from '$lib/components/navbar.svelte';
	import Question from '$lib/components/question.svelte';
    import PracticeSidebar from '$lib/components/practiceSidebar.svelte';

    let { data } = $props();

    let topic: string = $state("all");
    let difficulty: string = $state("all");
</script>

<div class="flex flex-col h-full">
	<Navbar currentPage="Practice"></Navbar>

    <div class="flex flex-1">
        <PracticeSidebar bind:topic={topic} bind:difficulty={difficulty}></PracticeSidebar>
        <div class="flex items-center justify-center flex-1">
            <div class="h-fit" style="width: 70%;">
                <Question topic={topic} difficulty={difficulty}></Question>
            </div>
        </div>
    </div>
</div>