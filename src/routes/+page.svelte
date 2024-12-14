<script lang="ts">
	import { Button } from "$lib/components/ui/button";
	import { Card, CardContent, CardFooter, CardHeader, CardTitle } from "$lib/components/ui/card";
	import { Trophy, CheckCircle, XCircle, Loader2 } from "lucide-svelte";
	import { fade, slide } from "svelte/transition";
    import { questions } from "$lib/data";
    import { Progress } from "$lib/components/ui/progress";


    let questions_data = questions;
	// Props for quiz questions
	// Get 10 random questions
	const randomQuestions = $derived([...questions_data].sort(() => Math.random() - 0.5).slice(0, 10));

	// Quiz state variables
	let answers = $state<(string | null)[]>(new Array(10).fill(null));
	let quizCompleted = $state(false);
	let score = $state(0);

	// Get citizenship verdict based on score
	const getCitizenshipVerdict = (score: number) => {
		if (score <= 3) {
			return {
				title: "Turista Impenitente! 🍍",
				description: "Hai totalizzato un punteggio così basso che persino i piccioni in Piazza San Marco ti guardano male. La tua conoscenza delle sagre italiane è pari a quella di chi ordina cappuccino dopo le 11. Dovrai fare molta penitenza a base di sagre paesane per rimediare a questo scempio!"
			};
		} else if (score <= 6) {
			return {
				title: "Aspirante Italiano 🍝",
				description: "Come la pasta scotta: ci sei quasi, ma non proprio. La tua conoscenza delle sagre è come un sugo fatto con il ketchup - si vede che ci provi, ma c'è ancora tanto da imparare. Frequenta più sagre e meno fast food!"
			};
		} else if (score <= 8) {
			return {
				title: "Quasi Cittadino 🍷",
				description: "Come un buon vino da tavola: non eccezionale, ma decisamente apprezzabile! La nonna ti guarderebbe con un misto di orgoglio e preoccupazione. Ancora qualche sagra e qualche festa patronale e potrai considerarti un vero italiano!"
			};
		} else {
			return {
				title: "Vero Italiano DOC 🇮🇹",
				description: "Complimenti! La tua conoscenza delle sagre è impeccabile come una carbonara della nonna! Potresti tranquillamente presiedere qualsiasi Pro Loco d'Italia. La patria è fiera di te, e ogni nonna del paese vorrebbe adottarti!"
			};
		}
	};

	/** Handle answer selection */
	const handleAnswerSelect = (questionIndex: number, answer: string) => {
		answers[questionIndex] = answer;
	};

	/** Submit quiz */
	const submitQuiz = () => {
		if (answers.includes(null)) return;
		score = answers.reduce((acc, answer, index) => {
			return answer === randomQuestions[index].risposta_corretta ? acc + 1 : acc;
		}, 0);
		quizCompleted = true;
		window.scrollTo({ top: 0, behavior: 'smooth' });
	};

	/** Reset quiz */
	const resetQuiz = () => {
		answers = new Array(10).fill(null);
		quizCompleted = false;
		score = 0;
		questions_data = [...questions_data];
		window.scrollTo({ top: 0, behavior: 'smooth' });
	};
</script>

<div class="min-h-screen sagre-bg">
	<div class="max-w-3xl mx-auto px-4 py-8 md:py-12 space-y-8">
		<!-- Quiz Header -->
		<div class="text-center space-y-4">
			<div class="float-animation">
				<Trophy class="h-16 w-16 mx-auto text-primary/80 mb-4" />
			</div>
			<h1 class="text-4xl font-bold tracking-tight bg-gradient-to-r from-green-600 via-primary to-red-600 bg-clip-text text-transparent pb-2">
				Sei degno della cittadinanza italiana?
			</h1>
			<p class="text-xl text-muted-foreground">Test ufficiale della conoscenza delle sagre italiane</p>
		</div>

		<!-- Quiz Content -->
		{#if !quizCompleted}
			<div class="space-y-8">
				<!-- Progress bar -->
				<div class="space-y-2 bg-card p-4 rounded-lg border border-primary/10">
					<Progress value={answers.filter(a => a !== null).length * 10} class="h-2" />
					<p class="text-sm text-muted-foreground text-center">
						{answers.filter(a => a !== null).length} di 10 domande completate
					</p>
				</div>

				{#each randomQuestions as question, index}
					<Card class="shadow-md hover:shadow-lg transition-shadow border-primary/10">
						<CardHeader>
							<CardTitle class="flex flex-col sm:flex-row sm:items-center gap-3">
								<span class="bg-primary/10 text-primary rounded-full w-8 h-8 flex items-center justify-center text-sm font-medium shrink-0">
									{index + 1}
								</span>
								<span class="text-base sm:text-lg">{question.domanda}</span>
							</CardTitle>
						</CardHeader>
						<CardContent>
							<div class="space-y-6">
								<div class="grid gap-3">
									{#each question.opzioni as option}
										<Button 
											variant={answers[index] === option ? "secondary" : "outline"} 
											class="justify-start h-auto p-4 text-left hover:bg-primary/5 transition-colors {answers[index] === option ? 'border-primary/50 shadow-sm' : ''} group"
											onclick={() => handleAnswerSelect(index, option)}
										>
											<span class="group-hover:translate-x-1 transition-transform duration-200">
												{option}
											</span>
										</Button>
									{/each}
								</div>
							</div>
						</CardContent>
					</Card>
				{/each}

				<Button 
					class="w-full text-lg py-6 shadow-lg hover:shadow-xl transition-all duration-300" 
					disabled={answers.includes(null)} 
					onclick={submitQuiz}
				>
					{#if answers.includes(null)}
						<Loader2 class="mr-2 h-5 w-5 animate-spin" />
						Rispondi a tutte le domande
					{:else}
						<span class="group-hover:scale-105 transition-transform duration-200">
							Verifica la tua italianità
						</span>
					{/if}
				</Button>
			</div>
		{:else}
			<!-- Quiz Results -->
			<Card class="shadow-lg border-primary/10">
				<CardHeader class="text-center pb-2 bg-primary/5 rounded-t-lg">
					<CardTitle class="text-3xl">
						<div in:fade class="flex items-center justify-center gap-3 mb-2">
							<Trophy class="h-10 w-10 text-yellow-500" />
							<span>Verdetto Finale</span>
						</div>
					</CardTitle>
				</CardHeader>
				<CardContent>
					<div class="space-y-8" in:slide>
						<div class="text-center space-y-6">
							<div class="inline-flex items-center justify-center">
								<div class="relative">
									<div class="absolute inset-0 flex items-center justify-center">
										<span class="text-4xl font-bold">{score}</span>
									</div>
									<svg class="w-32 h-32 transform -rotate-90">
										<circle
											cx="64"
											cy="64"
											r="56"
											stroke-width="8"
											stroke="currentColor"
											fill="transparent"
											class="text-muted/20"
										/>
										<circle
											cx="64"
											cy="64"
											r="56"
											stroke-width="8"
											stroke="currentColor"
											fill="transparent"
											stroke-dasharray={351.86}
											stroke-dashoffset={351.86 - (351.86 * score) / 10}
											class="text-primary transition-all duration-1000 ease-out"
										/>
									</svg>
								</div>
							</div>
							
							<div class="space-y-4 max-w-2xl mx-auto">
								<h2 class="text-2xl font-bold">{getCitizenshipVerdict(score).title}</h2>
								<p class="text-lg italic border-l-4 border-primary/20 pl-4 py-4 bg-primary/5 rounded-r pr-1">
									{getCitizenshipVerdict(score).description}
								</p>
							</div>
						</div>

						<div class="space-y-4">
							{#each randomQuestions as question, index}
								<div class="p-4 rounded-lg bg-muted/50 hover:bg-muted transition-colors">
									<div class="flex items-start justify-between gap-4">
										<div class="space-y-2">
											<p class="font-medium text-lg">{question.domanda}</p>
											<p class="text-sm text-muted-foreground">
												La tua risposta: <span class={answers[index] === question.risposta_corretta ? "text-success" : "text-destructive"}>{answers[index]}</span>
											</p>
											<p class="text-sm font-medium text-success">
												Risposta corretta: {question.risposta_corretta}
											</p>
										</div>
										{#if answers[index] === question.risposta_corretta}
											<CheckCircle class="h-6 w-6 text-success shrink-0" />
										{:else}
											<XCircle class="h-6 w-6 text-destructive shrink-0" />
										{/if}
									</div>
								</div>
							{/each}
						</div>
					</div>
				</CardContent>
				<CardFooter>
					<Button class="w-full text-lg py-6" onclick={resetQuiz}>
						<Trophy class="mr-2 h-5 w-5" />
						Riprova (Per veri patrioti)
					</Button>
				</CardFooter>
			</Card>
		{/if}
	</div>
</div>