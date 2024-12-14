<script lang="ts">
	import '../app.css';
	let { children } = $props();
</script>

<div class="min-h-screen w-full bg-background text-foreground relative">
	<!-- Decorative elements -->
	<div class="absolute inset-0 pointer-events-none overflow-hidden">
		<!-- Top section -->
		<div class="decoration" style="top: 5vh; right: 8vw">🍕</div>
		<div class="decoration" style="top: 15vh; left: 12vw">🍷</div>
		<div class="decoration" style="top: 35vh; left: 20vw">🧀</div>
		
		<!-- Middle section -->
		<div class="decoration" style="top: 65vh; right: 15vw">🍝</div>
		<div class="decoration" style="top: 85vh; left: 8vw">🫒</div>
		<div class="decoration" style="top: 105vh; right: 22vw">🍅</div>
		
		<!-- Lower section -->
		<div class="decoration" style="top: 135vh; right: 12vw">🍷</div>
		<div class="decoration" style="top: 165vh; left: 15vw">🍕</div>
		<div class="decoration" style="top: 195vh; right: 20vw">🧀</div>
		
		<!-- Bottom section -->
		<div class="decoration" style="top: 225vh; left: 10vw">🍝</div>
		<div class="decoration" style="top: 255vh; right: 15vw">🫒</div>
		<div class="decoration" style="top: 285vh; left: 18vw">🍅</div>
		
		<!-- Extra bottom section -->
		<div class="decoration" style="top: 315vh; right: 12vw">🍕</div>
		<div class="decoration" style="top: 345vh; left: 20vw">🍷</div>
		<div class="decoration" style="top: 375vh; right: 8vw">🧀</div>
		<div class="decoration" style="top: 405vh; left: 15vw">🍝</div>

		<!-- Final bottom section -->
		<div class="decoration" style="top: 435vh; right: 18vw">🫒</div>
		<div class="decoration" style="top: 465vh; left: 12vw">🍅</div>
		<div class="decoration" style="top: 495vh; right: 15vw">🍕</div>
		<div class="decoration" style="top: 525vh; left: 8vw">🍷</div>
	</div>

	<!-- Main content -->
	<main class="relative z-10 p-4 md:p-8">
		<div class="container mx-auto max-w-3xl">
			{@render children()}
		</div>
	</main>
</div>