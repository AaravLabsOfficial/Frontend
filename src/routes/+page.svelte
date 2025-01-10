<script lang="ts">
	import Navbar from '$lib/components/navbar.svelte';
	import Question from '$lib/components/question.svelte';
</script>

<Navbar currentPage="Home"></Navbar>

<table style="width: 100%;">
	<tbody class="maxSize">
		<tr class="h-16"></tr>
		<tr class="maxSize" style="height:330px">
			<td style="width: 49.99%; height: 100%;">
				<div class="m-auto w-fit text-center">
					<h1 class="text-6xl font-extrabold tracking-tight drop-shadow-glow lg:text-5xl">
						Welcome to Aarav Labs
					</h1>
					<h1 class="m-auto mt-8 w-3/4 text-xl font-semibold tracking-tight lg:text-2xl">
						Learn Prealgebra, Algebra, Geometry, Counting & Probability, Number Theory, Precalculus,
						and more with our large set of questions and AI analysis.
					</h1>
				</div>
			</td>
			<td style="width: 49.99%; height: 100%;">
				<div class="m-auto h-full w-3/4 text-start">
					<Question topic="all" difficulty="all" fake={true}></Question>
				</div>
			</td>
		</tr>
		<tr class="h-32"></tr>
		<tr class="maxSize" style="height:330px">
			<td style="width: 49.99%; height: 100%;">
				<div class="m-auto h-full w-3/4 text-start">
					<Question
						topic="all"
						difficulty="all"
						fake={true}
						solvedProp={true}
						questionProp="Find $x$, where $x$ is a square number which is divisible by four, and between 39 and 80."
						fullAnswerProp={String.raw`We know that $x$ is between 39 and 80, and since $6^2 = 36 < 39$ and $9^2 = 81 > 80$, this means that $6^2 < x < 9^2$. This leaves us with two possibilities for $x$, which are $7^2 = 49$, and $8^2 = 64$. We then see that only 64 is divisible by four, so $x =$ $\boxed{64}$.`}
						stepsProp={"First, list the squares between $39$ and $80$:\n $49, 64$\n\n" + String.raw`Since $\boxed{64}$ is $4 \cdot 16$, it is divisible by 4.`}
						answerProp="$64$"
                        stepsCorrectProp={true}
                        answerCorrectProp={true}
						stepsAnalysisProp="Your steps are correct and well-explained. You correctly identified the two square numbers between 39 and 80, and then you correctly identified that 64 is divisible by 4."
                        answerAnalysisProp="Your answer is correct!"
					></Question>
				</div>
			</td>
			<td style="width: 49.99%; height: 100%;">
				<div class="m-auto w-fit text-center">
					<h1 class="text-5xl font-extrabold tracking-tight lg:text-4xl">AI Answer Analysis</h1>
					<h1 class="m-auto mt-8 w-3/4 text-xl font-semibold tracking-tight lg:text-2xl">
						AI answer analysis makes you show your work and helps you if you get stuck in any step.
					</h1>
				</div>
			</td>
		</tr>
    </tbody>
</table>
<table style="width: 100%;">
	<tbody class="maxSize">
		<tr class="maxSize" style="height:330px">
			<td style="height: 100%;">
				<div class="m-auto w-fit text-center">
					<h1 class="text-5xl font-extrabold tracking-tight lg:text-4xl">
						Great Questions and Explaination
					</h1>
					<h1 class="m-auto mt-8 w-3/4 text-xl font-semibold tracking-tight lg:text-2xl">
						Over 1200 questions in all topics with amazing explaination and rich formatting.
					</h1>
				</div>
			</td>
		</tr>
	</tbody>
</table>
