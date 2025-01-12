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
        <!-- Choose topic and difficulty here and set it to a variable -->
        <PracticeSidebar bind:topic={topic} bind:difficulty={difficulty}></PracticeSidebar>
        <div class="flex items-center justify-center flex-1">
            <div class="h-fit" style="width: 70%;">
                <!-- Then give the topic and difficulty to Question whenever it updates -->
                <Question topic={topic} difficulty={difficulty}></Question>
            </div>
        </div>
    </div>
</div>