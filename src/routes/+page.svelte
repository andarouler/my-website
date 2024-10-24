<script>
	import { onMount, tick } from 'svelte';
	import { gsap } from 'gsap';
	import { ScrollTrigger } from 'gsap/dist/ScrollTrigger';

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

	// Split the heading text into individual characters
	let headingText = 'Hello'.split('');

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
		setInterval(cycleSentences, 5000); // Cycle every 5 seconds

		// Special bounce effect for the h2 in the #about section
		gsap.fromTo(
			'#about h2',
			{
				opacity: 0,
				scale: 0.5,
				y: -30
			},
			{
				opacity: 1,
				scale: 1,
				y: 0,
				duration: 1.5,
				ease: 'bounce.out',
				onComplete: () => {
					let vibrationDuration = 0.1;

					// Vibration effect after the bounce animation
					gsap.to('#about h2', {
						duration: vibrationDuration,
						repeat: 10,
						yoyo: true,
						x: '+=2',
						ease: 'power1.inOut',
						onRepeat: function () {
							vibrationDuration += 0.02;
							this.duration(vibrationDuration);
						},
						onComplete: () => {
							function animateLettersRandomly() {
								const letters = document.querySelectorAll('#about h2 span');

								gsap.fromTo(
									letters,
									{ x: 50, opacity: 0 }, // Startzustand
									{
										x: 0,
										opacity: 1,
										duration: 1,
										stagger: {
											amount: 1, // Zeit zwischen den Buchstaben
											from: 'start' // Animation beginnt von links nach rechts
										},
										ease: 'power2.out',
										onComplete: () => {
											// Nach der Animation, eine zufällige Zeit warten und dann die Animation erneut ausführen
											const randomDelay = Math.random() * 10 + 5; // Zufällige Verzögerung zwischen 2 und 7 Sekunden
											gsap.delayedCall(randomDelay, animateLettersRandomly);
										}
									}
								);
							}

							// Starte die erste Animation
							animateLettersRandomly();
						}
					});
				}
			}
		);

		// Apply scroll-triggered animations to the other h2 elements
		const headings = document.querySelectorAll('section:not(#about) h2');
		headings.forEach((heading) => {
			gsap.from(heading, {
				opacity: 0,
				y: 50,
				duration: 1,
				ease: 'power2.out',
				scrollTrigger: {
					trigger: heading,
					start: 'top 80%',
					toggleActions: 'play none none none'
				}
			});
		});
	});
</script>

<main>
	<!-- Über mich -->
	<section id="about">
		<h2>
			{#each headingText as letter}
				<span>{letter}</span>
			{/each}
		</h2>
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
