<!-- Math practice component for rendering and evaluating math problems -->
<!-- Imports required UI components and utilities -->
<script lang="ts">
	import * as Card from '$lib/components/ui/card';
	import { Button } from '$lib/components/ui/button';
	import { Input } from '$lib/components/ui/input';
	import katex from 'katex';
	import { browser } from '$app/environment';
	import { onMount } from 'svelte';
	import { PUBLIC_API_URL } from '$env/static/public';

	// Renders a single math expression using KaTeX
	// @param input - The math expression to render
	// @param displayMode - Whether to render in display (block) or inline mode
	function renderMathSegment(input: string, displayMode: boolean): string {
		return katex.renderToString(String.raw({ raw: input }), {
			displayMode: displayMode,
			throwOnError: false
		});
	}

	// Processes text containing multiple math expressions and converts them to rendered HTML
	// Handles both inline ($, \(\)) and display ($$, \[\]) math
	function renderMath(input: string): string {
		// Regular expressions for finding different types of math delimiters
		let inlineRegex = [/(?<!\\)\$\s*[\s\S]*?(?<!\\)\$/g, /(?<!\\)\\\(\s*[\s\S]*?(?<!\\)\\\)/g];
		let displayRegex = [/(?<!\\)\$\$\s*[\s\S]*?(?<!\\)\$\$/g, /(?<!\\)\\\[\s*[\s\S]*?(?<!\\)\\\]/g];

		// Convert newlines to HTML breaks
		let output = input.replace(/\n/g, '<br />');

		// Process \(...\) inline math
		let match = undefined;
		while ((match = inlineRegex[1].exec(output)) != null) {
			const startIndex = match.index;
			const endIndex = match.index + match[0].length;

			let value = renderMathSegment(match[0].substring(2, match[0].length - 2), false);
			output = output.substring(0, startIndex) + value + output.substring(endIndex, output.length);
		}

		// Process $$ display math
		match = undefined;
		while ((match = displayRegex[0].exec(output)) != null) {
			const startIndex = match.index;
			const endIndex = match.index + match[0].length;

			let value = renderMathSegment(match[0].substring(2, match[0].length - 2), true);
			output = output.substring(0, startIndex) + value + output.substring(endIndex, output.length);
		}

		// Process \[...\] display math
		match = undefined;
		while ((match = displayRegex[1].exec(output)) != null) {
			const startIndex = match.index;
			const endIndex = match.index + match[0].length;

			let value = renderMathSegment(match[0].substring(2, match[0].length - 2), true);
			output = output.substring(0, startIndex) + value + output.substring(endIndex, output.length);
		}

		// Process $ inline math
		match = undefined;
		while ((match = inlineRegex[0].exec(output)) != null) {
			const startIndex = match.index;
			const endIndex = match.index + match[0].length;

			let value = renderMathSegment(match[0].substring(1, match[0].length - 1), false);
			output = output.substring(0, startIndex) + value + output.substring(endIndex, output.length);
		}

		return output;
	}

	// State variables for the question and user input
	let question = $state('');
	let renderedQuestion: string = $derived.by((): string => renderMath(question));

	let steps = $state('');
	let renderedSteps: string = $derived.by((): string => renderMath(steps));

	let answer = $state('');
	let renderedAnswer: string = $derived.by((): string => renderMath(answer));

	let fullAnswer = $state('');
	let renderedFullAnswer: string = $derived.by((): string => renderMath(fullAnswer));

	// State variables for analysis and feedback
	let stepsAnalysis = $state('');
	let renderedStepsAnalysis: string = $derived.by((): string => renderMath(stepsAnalysis));

	let answerAnalysis = $state('');
	let renderedAnswerAnalysis: string = $derived.by((): string => renderMath(answerAnalysis));

	// Component props with default values
	let {
		topic = $bindable('all'),
		difficulty = $bindable('all'),
		fake = false,
		solvedProp = undefined,
		questionProp = undefined,
		fullAnswerProp = undefined,
		stepsProp = undefined,
		answerProp = undefined,
		stepsAnalysisProp = undefined,
		answerAnalysisProp = undefined,
		stepsCorrectProp = undefined,
		answerCorrectProp = undefined
	} = $props<{
		topic: string;
		difficulty: string;
		fake?: boolean;
		solvedProp?: boolean;
		questionProp?: string;
		fullAnswerProp?: string;
		stepsProp?: string;
		answerProp?: string;
		stepsAnalysisProp?: string;
		answerAnalysisProp?: string;
		stepsCorrectProp?: boolean;
		answerCorrectProp?: boolean;
	}>();

	// Available math topics and their display names
	const acceptedTopics = [
		'all',
		'algebra',
		'intermediate_algebra',
		'prealgebra',
		'geometry',
		'number_theory',
		'counting_probability',
		'precalculus'
	];
	const convertedTopics = [
		'all',
		'Algebra',
		'Intermediate Algebra',
		'Prealgebra',
		'Geometry',
		'Number Theory',
		'Counting & Probability',
		'Precalculus'
	];

	// State variables for tracking topic and difficulty changes
	let renderedTopic = $state();
	let renderedDifficulty = $state();

	let previousTopic = $state(topic);
	let previousDifficulty = $state(difficulty);

	// State variables for correctness and error tracking
	let stepsCorrect: boolean | undefined = $state(undefined);
	let answerCorrect: boolean | undefined = $state(undefined);

	let stepsError: boolean = $state(false);
	let answerError: boolean = $state(false);

	// Fetches a new question from the API based on topic and difficulty
	const renderQuestion = () => {
		if (loadQuestion) {
			fetch(`${PUBLIC_API_URL}/?topic=${topic}&difficulty=${difficulty}`)
				.then((response) => {
					return response.json();
				})
				.then((body) => {
					// Skip questions with complex formatting
					if (
						body.question.includes('[asy') ||
						body.question.includes('\\begin{') ||
						body.answer.includes('[asy') ||
						body.answer.includes('\\begin{')
					) {
						renderQuestion();
					} else {
						question = body.question;
						renderedTopic = body.topic;
						renderedDifficulty = body.difficulty;
						fullAnswer = body.answer;
					}
				});
		}
	};

	// Effect to handle topic changes
	$effect(() => {
		if (topic != previousTopic) {
			renderQuestion();
			previousTopic = topic;
		}
	});

	// Effect to handle difficulty changes
	$effect(() => {
		if (difficulty != previousDifficulty) {
			renderQuestion();
			previousDifficulty = difficulty;
		}
	});

	let solved = $state(false);
	let loadQuestion = true;

	// Effect to handle fake/mock mode
	$effect(() => {
		if (fake) {
			if (solvedProp) {
				solved = solvedProp;
			}
			if (questionProp) {
				question = questionProp;
			}
			if (fullAnswerProp) {
				fullAnswer = fullAnswerProp;
			}
			if (stepsProp) {
				steps = stepsProp;
			}
			if (answerProp) {
				answer = answerProp;
			}
			if (stepsAnalysisProp) {
				stepsAnalysis = stepsAnalysisProp;
			}
			if (answerAnalysisProp) {
				answerAnalysis = answerAnalysisProp;
			}
			if (stepsCorrectProp) {
				stepsCorrect = stepsCorrectProp;
			}
			if (answerCorrectProp) {
				answerCorrect = answerCorrectProp;
			}

			if (
				questionProp ||
				answerProp ||
				answerAnalysisProp ||
				stepsAnalysisProp ||
				fullAnswerProp ||
				stepsProp ||
				stepsCorrectProp ||
				answerCorrectProp
			) {
				loadQuestion = false;
			}
		}
	});

	// Initialize component by loading first question
	onMount(renderQuestion);

	// Handles submission of user's solution
	const solveQuestion = async () => {
		if (!fake) {
			if (
				steps.replaceAll(' ', '').replaceAll(/\n/g, '') == '' ||
				answer.replaceAll(' ', '').replaceAll(/\n/g, '') == ''
			) {
				if (steps.replaceAll(' ', '').replaceAll(/\n/g, '') == '') {
					// Mark steps as empty
					stepsError = true;
				} else {
					stepsError = false;
				}
				if (answer.replaceAll(' ', '').replaceAll(/\n/g, '') == '') {
					// Mark answer as empty
					answerError = true;
				} else {
					answerError = false;
				}
			} else {
				// Submit solution to API for analysis
				let response = await fetch(PUBLIC_API_URL + '/aiAnalysis', {
					method: 'POST',
					headers: {
						'Content-Type': 'application/json'
					},
					body: JSON.stringify({
						question: question,
						correct_answer: fullAnswer,
						user_steps: steps,
						user_answer: answer
					})
				});

				const responseData = await response.json();
				console.log('Success: ', responseData);

				// Update feedback and correctness states
				stepsAnalysis = responseData.stepsAnalysis;
				answerAnalysis = responseData.answerAnalysis;
				stepsCorrect = responseData.stepsCorrect;
				answerCorrect = responseData.answerCorrect;

				// Show solution review
				solved = true;
			}
		} else {
			console.log('FAKEEE');
			location.replace('/practice');
		}
	};

	// Handles progression to next question
	const nextQuestion = () => {
		if (!fake) {
			// Reset all states and load new question
			solved = false;
			stepsAnalysis = '';
			answerAnalysis = '';
			stepsCorrect = undefined;
			answerCorrect = undefined;
			steps = '';
			answer = '';
			renderQuestion();
		} else {
			location.replace('/practice');
		}
	};
</script>

<!-- Load KaTeX CSS for math rendering -->
{#if browser}
	<link
		rel="stylesheet"
		href="https://cdn.jsdelivr.net/npm/katex@0.16.18/dist/katex.min.css"
		integrity="sha384-veTAhWILPOotXm+kbR5uY7dRamYLJf58I7P+hJhjeuc7hsMAkJHTsPahAl0hBST0"
		crossorigin="anonymous"
	/>
{/if}

<!-- Practice mode UI -->
{#if !solved}
	<Card.Root class="drop-shadow-glow">
		<Card.Header>
			<Card.Title class="font-normal">{@html renderedQuestion}</Card.Title>
			<Card.Description>{renderedTopic} - Level {renderedDifficulty}</Card.Description>
		</Card.Header>
		<Card.Content>
			<!-- Skip question button -->
			<Button variant="default" onclick={renderQuestion}>Skip Question</Button><br /><br />

			<!-- Steps input section -->
			<span class="font-semibold">Steps: </span> <br />
			<textarea
				bind:value={steps}
				class="flex min-h-[60px] w-full rounded-md border border-input bg-transparent px-3 py-2 text-base shadow-sm placeholder:text-muted-foreground focus-visible:outline-none focus-visible:ring-1 focus-visible:ring-ring disabled:cursor-not-allowed disabled:opacity-50 md:text-sm"
			>
			</textarea>
			<!-- Steps error message -->
			{#if stepsError}
				<div class="mt-3 text-sm text-red-500">Steps can't be empty!</div>
			{/if}
			<!-- Steps preview with LaTeX rendering -->
			{#if renderedSteps.replaceAll(' ', '').replaceAll('<br/>', '') != ''}
				<br />
				<span>Preview: </span>
				<div
					class="mb-4 w-full rounded-md border bg-transparent px-3 py-2 text-base shadow-sm md:text-sm"
				>
					<span contenteditable="false">{@html renderedSteps}</span>
				</div>
			{/if}

			<!-- Answer input section -->
			<br />
			<span class="font-semibold">Answer: </span> <br />
			<Input bind:value={answer}></Input>
			<!-- Answer error message -->
			{#if answerError}
				<div class="mt-3 text-sm text-red-500">Answer can't be empty!</div>
			{/if}
			<!-- Answer preview with LaTeX rendering -->
			{#if renderedAnswer.replaceAll(' ', '').replaceAll('<br/>', '') != ''}
				<br />
				<span>Preview: </span>
				<div
					class="mb-4 w-full rounded-md border bg-transparent px-3 py-2 text-base shadow-sm md:text-sm"
				>
					<span contenteditable="false">{@html renderedAnswer}</span>
				</div>
			{/if}
		</Card.Content>
		<Card.Footer>
			<Button variant="default" onclick={solveQuestion}>Submit</Button>
		</Card.Footer>
	</Card.Root>
{:else}
	<!-- Solution review mode UI -->
	<Card.Root class="drop-shadow-glow">
		<Card.Header>
			<!-- Correctness indicator -->
			{#if answerCorrect}
				<Card.Title class="text-xl">Correct! Way to go!</Card.Title>
			{:else}
				<Card.Title class="text-xl">Incorrect! Oops!</Card.Title>
			{/if}
		</Card.Header>
		<Card.Content>
			<!-- Original question display -->
			<span class="font-semibold">Question: </span>
			<div
				class="mb-4 w-full rounded-md border bg-transparent px-3 py-2 text-base shadow-sm md:text-sm"
			>
				<span contenteditable="false">{@html renderedQuestion}</span>
			</div>

			<!-- User's steps with feedback -->
			<span class="font-semibold">Your Steps: </span>
			<div
				class="mb-4 w-full rounded-md border bg-transparent px-3 py-2 text-base shadow-sm md:text-sm"
			>
				<span contenteditable="false">{@html renderedSteps}</span>
				<br /><br />
				<!-- Feedback container with conditional styling based on correctness -->
				<div
					class="rounded-lg p-3 {stepsCorrect != undefined
						? stepsCorrect == true
							? 'bg-green-300'
							: 'bg-red-300'
						: ''}"
				>
					<span contenteditable="false">{@html renderedStepsAnalysis}</span>
				</div>
			</div>

			<!-- User's answer with feedback -->
			<span class="font-semibold">Your Answer: </span> <br />
			<div
				class="mb-4 w-full rounded-md border bg-transparent px-3 py-2 text-base shadow-sm md:text-sm"
			>
				<span contenteditable="false">{@html renderedAnswer}</span>
				<br /><br />
				<!-- Feedback container with conditional styling based on correctness -->
				<div
					class="rounded-lg p-3 {answerCorrect != undefined
						? answerCorrect == true
							? 'bg-green-300'
							: 'bg-red-300'
						: ''}"
				>
					<span contenteditable="false">{@html renderedAnswerAnalysis}</span>
				</div>
			</div>

			<!-- Complete solution display -->
			<span class="font-semibold">Full Answer: </span> <br />
			<div
				class="mb-4 w-full rounded-md border bg-transparent px-3 py-2 text-base shadow-sm md:text-sm"
			>
				<span contenteditable="false">{@html renderedFullAnswer}</span>
			</div>
		</Card.Content>
		<Card.Footer>
			<!-- Next question button -->
			<Button variant="default" onclick={nextQuestion}>Next Question</Button>
		</Card.Footer>
	</Card.Root>
{/if}
