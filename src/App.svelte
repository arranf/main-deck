<script>
	let cards = ['+5', '+4', '+3', '+2', '+1', '0', '-1', '-2', '-3', '-4', '-5'];
	let usedCards = [];
	let currentCard;
	let showCards = false;

	cards = shuffle(cards);

	function handleClick() {
		if (cards.length == 0) {
			reshuffleDeck();
		}
		const card = cards.pop();
		cards = cards;
		currentCard = card;
		usedCards = [...usedCards, card];
	}

	function reshuffleDeck() {
		cards = shuffle(['+5', '+4', '+3', '+2', '+1', '0', '-1', '-2', '-3', '-4', '-5'])
		usedCards = [];
		currentCard = undefined;
	}

	function addCard(card) {
		usedCards = usedCards.filter(item => item !== card)
		cards = shuffle([...cards, card])
	}

	function discardCard(card) {
		cards = cards.filter(item => item !== card)
		usedCards = [...usedCards, card];
	}

	function toggleCards() {
		showCards = !showCards;
	}

	/**
	 * Shuffles array in place. ES6 version
	 * @param {Array} a items An array containing the items.
	 */
	function shuffle(a) {
		for (let i = a.length - 1; i > 0; i--) {
			const j = Math.floor(Math.random() * (i + 1));
			[a[i], a[j]] = [a[j], a[i]];
		}
		return a;
	}
</script>

<main>
	<p>
		Draw from your main deck
	</p>
	<button on:click={handleClick}>Draw a card!</button>
	<button on:click={reshuffleDeck}>Reshuffle the deck</button>
	<button on:click={toggleCards}>
		{#if showCards}
			Hide the cards in my deck
		{:else}
			Show the cards in my deck
		{/if}
	</button>

	<div>
		<h3>Cards Left in Deck</h3>
		{cards.length}
		
		<h3>Current Card</h3>

	{#if currentCard}
		<span class="current-card">{currentCard}</span>	
	{:else}
		<span>No card drawn from the deck yet</span>
	{/if}
	</div>

	<div>
		<h3>Used Cards</h3>
		{#if usedCards.length > 0}
		<ul>
			{#each usedCards.reverse() as card}
				<li>{card} <button class="small-button" on:click={addCard(card)}>Add to deck</button></li> 
			{/each}
		</ul>
		{/if}
	</div>


	{#if showCards}
	<div>
		<h3>Cards In Deck</h3>
				{#if cards.length > 0}
				<ul>
					{#each cards as card}
						<li>{card} <button class="small-button" on:click={discardCard(card)}>Discard card</button></li> 
					{/each}
				</ul>
				{/if}
	</div>
	{/if}
</main>

<style>
	main {
		text-align: center;
		padding: 1em;
		max-width: 240px;
		margin: 0 auto;
	}

	.current-card {
	  font-weight: 900;
	  font-size: 140%;
	}

	.small-button {
		font-size: 85%;
		display: inline-block;
		padding: 5px;
	}

	ul {	
	  list-style-type: none;
	}

	@media (min-width: 640px) {
		main {
			max-width: none;
		}
	}
</style>