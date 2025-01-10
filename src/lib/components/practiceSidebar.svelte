<script lang="ts">
	import * as Card from '$lib/components/ui/card';
	import * as Select from '$lib/components/ui/select/index.js';

	const topics = [
		{ value: 'algebra', label: 'Algebra' },
		{ value: 'intermediate_algebra', label: 'Intermediate Algebra' },
		{ value: 'prealgebra', label: 'Prealgebra' },
		{ value: 'geometry', label: 'Geometry' },
		{ value: 'number_theory', label: 'Number Theory' },
		{ value: 'counting_probability', label: 'Counting and Probability' },
		{ value: 'precalculus', label: 'Precalculus' },
		{ value: 'all', label: 'All' }
	];

	let { topic = $bindable('all'), difficulty = $bindable('all') } = $props<{
		topic: string;
		difficulty: string;
	}>();

	const selectedTopicName = $derived(
		topic ? topics.find((topics) => topics.value === topic)?.label : 'Select a topic'
	);

	const difficulties = [
		{ value: '1', label: '1' },
		{ value: '2', label: '2' },
		{ value: '3', label: '3' },
		{ value: '4', label: '4' },
		{ value: '5', label: '5' },
		{ value: 'all', label: 'All' }
	];

	const selectedDifficulty = $derived(
		difficulty
			? difficulties.find((difficulties) => difficulties.value === difficulty)?.label
			: 'Select a difficulty'
	);
</script>

<Card.Root class="m-8 min-w-96 max-w-96 drop-shadow-glow">
	<Card.Header>
		<Card.Title class="text-2xl font-bold">Questions</Card.Title>
	</Card.Header>
	<Card.Content>
		<span class="text-lg font-semibold">Topic: </span> <br />
		<Select.Root type="single" onValueChange={(v) => (topic = v)}>
			<Select.Trigger aria-label="Select a topic">
				{selectedTopicName}
			</Select.Trigger>
			<Select.Content>
				<Select.Item value="all">All</Select.Item>
				<Select.Item value="algebra">Algebra</Select.Item>
				<Select.Item value="intermediate_algebra">Intermediate Algebra</Select.Item>
				<Select.Item value="prealgebra">Prealgebra</Select.Item>
				<Select.Item value="geometry">Geometry</Select.Item>
				<Select.Item value="number_theory">Number Theory</Select.Item>
				<Select.Item value="counting_probability">Counting and Probability</Select.Item>
				<Select.Item value="precalculus">Precalculus</Select.Item>
			</Select.Content>
		</Select.Root>

		<br />
		<span class="text-lg font-semibold">Question Difficulty: </span> <br />

		<Select.Root type="single" onValueChange={(v) => (difficulty = v)}>
			<Select.Trigger aria-label="Select a difficulty">
				{selectedDifficulty}
			</Select.Trigger>
			<Select.Content>
				<Select.Item value="all">All</Select.Item>
				<Select.Item value="1">1</Select.Item>
				<Select.Item value="2">2</Select.Item>
				<Select.Item value="3">3</Select.Item>
				<Select.Item value="4">4</Select.Item>
				<Select.Item value="5">5</Select.Item>
			</Select.Content>
		</Select.Root>
	</Card.Content>
</Card.Root>
