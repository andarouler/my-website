<script>
	import { onMount, tick } from 'svelte';
	import { gsap } from 'gsap';

	import { ScrollTrigger } from 'gsap/dist/ScrollTrigger';

	// Register ScrollTrigger plugin
	gsap.registerPlugin(ScrollTrigger);
	import Card from '$lib/Card.svelte';

	// import personal stuff
	import profilePic from '$lib/images/IMG_0126.JPG';
	import personalData from '$lib/personal_data.json';

	// Variablen und Daten für "Über mich" und "Erfahrungen"
	let name = 'Andy';
	let aboutMe = `I'm ${name}`;
	let { aboutMeExtended, experiences, education } = personalData;

	// Split the aboutMeExtended text into sentences
	let sentences = aboutMeExtended.text.split('. ').filter((sentence) => sentence.length > 0);

	// State for the current sentence
	let currentSentence = sentences[0];
	let currentIndex = 0;

	// Function to cycle through sentences
	function cycleSentences() {
		currentIndex = (currentIndex + 1) % sentences.length;
		currentSentence = sentences[currentIndex];

		// Wait for DOM update, then apply GSAP animation
		tick().then(() => {
			gsap.fromTo(
				'.sentence',
				{ opacity: 0, y: 30, letterSpacing: '2px', scale: 0.9 },
				{ opacity: 1, y: 0, letterSpacing: 'normal', scale: 1, duration: 1.5, ease: 'power3.out' }
			);
		});
	}

	onMount(() => {
		cycleSentences(); // Initialize the first animation
		setInterval(cycleSentences, 5000); // Cycle every 4 seconds

		// Select all h2 elements individually and animate them with ScrollTrigger
		const headings = document.querySelectorAll('h2');
		headings.forEach((heading) => {
			gsap.from(heading, {
				opacity: 0,
				y: 50,
				duration: 1,
				ease: 'power2.out',
				scrollTrigger: {
					trigger: heading, // Trigger for each individual h2
					start: 'top 80%', // Start the animation when the top of the heading reaches 80% of the viewport
					toggleActions: 'play none none none' // Play once when in view
				}
			});
		});
	});
</script>

<main>
	<!-- Über mich -->
	<section id="about">
		<h2>Hello</h2>
		<img src={profilePic} alt="profile" />
		<p>{aboutMe} - <span class="sentence">{currentSentence}</span></p>
	</section>

	<!-- Erfahrungen -->
	<section id="experiences">
		<h2>Experiences</h2>
		{#each experiences as experience}
			<Card title={experience.title} timeline={experience.timeline}>
				<p><strong>Firma:</strong> {experience.company}</p>
				<p>{experience.description}</p>
			</Card>
		{/each}
	</section>

	<!-- Ausbildung -->
	<section id="education">
		<h2>Education</h2>
		{#each education as edu}
			<Card title={edu.degree} timeline={edu.timeline}>
				<p><strong>University:</strong>{edu.school}</p>
				{#if edu.thesis}
					<p><strong>Thesis:</strong>{edu.thesis}</p>
				{/if}
			</Card>
		{/each}
	</section>

	<!-- Contact -->
	<section id="contact">
		<h2>Contact me</h2>
		<p>Lorem ipsum</p>
		<p>Lorem ipsum</p>
		<p>Lorem ipsum</p>
		<p>Lorem ipsum</p>
		<p>Lorem ipsum</p>
	</section>
</main>
