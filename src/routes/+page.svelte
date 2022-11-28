<script lang="ts">
	import { to_number } from 'svelte/internal';

	function sleep(ms: number) {
		return new Promise((resolve) => setTimeout(resolve, ms));
	}

	let a = 1;
	let b = 2;

	async function add() {
		// p1container.appendChild(card);
		console.log('hello');
	}

	let blacklist: string[] = [];

	let cards: string[] = ['2', '3', '4', '5', '6', '7', '8', '9', '10', 'J', 'Q', 'K', 'A'];

	let cardsMap: any = {
		'2': 2,
		'3': 3,
		'4': 4,
		'5': 5,
		'6': 6,
		'7': 7,
		'8': 8,
		'9': 9,
		'10': 10,
		J: 10,
		Q: 10,
		K: 10,
		A: [1, 11]
	};
	let p1score: number = 0;
	let p1wins: number = 0;
	let p1cards: any = [];

	let p2score: number = 0;
	let p2wins: number = 0;
	let p2cards: any = [];

	let draws: number = 0;

	async function bjHit() {
		// alertplayers();

		let ls = getCard();
		let num = ls[0];
		let suit = ls[1];

		let card: string = `${suit}-${num}`;
		p1cards.push({ suit, num });
		p1cards = p1cards;
		console.log(p1cards);
		p1score = addscore(p1score, num);

		if (p1score > 21) {
			p2wins += 1;
		}
		// updateScore(player1, card);

		// //console.log(player1['score'])

		// showscore(player1);
	}
	async function bjStand() {
		let a = 1;
		let b = 2;
		while (a < b) {
			sleep(8000);
			let ls = getCard();

			let num = ls[0];
			let suit = ls[1];

			let blackcard = num + suit;
			p2cards.push({ suit, num });
			p2cards = p2cards;
			p2score = addscore(p2score, num);

			if (p2score > 21) {
				p2score = 0;
				a = 3;
				break;
			}
			if (p2score >= p1score) {
				a = 3;
				break;
			}
		}
		endround();
	}
	function endround() {
		if (p1score > p2score) {
			p1wins++;
			p1wins = p1wins;
		}

		if (p1score < p2score) {
			p2wins++;
			p2wins = p2wins;
		}

		if (p1score == p2score) {
			draws++;
			draws = draws;
		}
	}

	function bjShuffle() {
		blacklist = [];
	}

	function randomNum() {
		let random = Math.floor(Math.random() * 13);
		let rancard: string = cards[random];
		return rancard;
	}

	function randomSuit() {
		let suit = ['H', 'S', 'D', 'C'];
		let random = Math.floor(Math.random() * 4);
		return suit[random];
	}

	function getCard(): any {
		let num = randomNum();
		let suit = randomSuit();
		let card: string = `${suit}-${num}`;
		if (blacklist.includes(card)) {
			return getCard();
		} else {
			blacklist.push(card);

			return [num, suit];
		}
	}
	function addscore(playerscore: number, num: number) {
		let scoreUp = cardsMap[num];
		if (typeof scoreUp == 'object') {
			if (playerscore + scoreUp[1] <= 21) {
				return (playerscore += scoreUp[1]);
			} else {
				return (playerscore += scoreUp[0]);
			}
		} else {
			return (playerscore += scoreUp);
		}
	}
</script>

<button class="font-bold text-sky-400" on:click={bjHit}>Hit</button>
<button class="font-bold text-sky-400" on:click={bjStand}>Stand</button>

<p>p1score: {p1score}</p>
<p>p2score: {p2score}</p>
<p>p1wins: {p1wins}</p>
<p>p2wins: {p2wins}</p>
<p>draws : {draws}</p>
<div class="flex flex-row">
	{#each p1cards as { suit, num }}
		<div class="max-w-sm rounded overflow-hidden shadow-lg h-48 w-28 mx-auto sticky">
			<span class="flex flex-row">
				<svg class="icon icon-{suit}"><use xlink:href="#icon-{suit}" /></svg>
				<div class="font-bold text-xl text-white ">{num}</div>
			</span>

			<img src="VectorWheat.svg" alt="My Happy SVG" />
			<span class="flex flex-row absolute bottom-0 left-100 right-0">
				<svg class="icon icon-{suit}"><use xlink:href="#icon-{suit}" /></svg>
				<div class="font-bold text-xl text-white ">{num}</div>
			</span>
		</div>
	{/each}
</div>

<div class="flex flex-row">
	{#each p2cards as { suit, num }}
		<div class="max-w-sm rounded overflow-hidden shadow-lg h-48 w-28 mx-auto sticky">
			<span class="flex flex-row">
				<svg class="icon icon-{suit}"><use xlink:href="#icon-{suit}" /></svg>
				<div class="font-bold text-xl text-white ">{num}</div>
			</span>

			<img src="VectorWheat.svg" alt="My Happy SVG" />
			<span class="flex flex-row absolute bottom-0 left-100 right-0">
				<svg class="icon icon-{suit}"><use xlink:href="#icon-{suit}" /></svg>
				<div class="font-bold text-xl text-white ">{num}</div>
			</span>
		</div>
	{/each}
</div>

<div class="max-w-sm rounded overflow-hidden shadow-lg h-48 w-28 mx-auto sticky">
	<span class="flex flex-row">
		<svg class="icon icon-S"><use xlink:href="#icon-S" /></svg>
		<div class="font-bold text-xl text-white ">K</div>
	</span>

	<img src="VectorWheat.svg" alt="My Happy SVG" />
	<span class="flex flex-row absolute bottom-0 left-100 right-0">
		<svg class="icon icon-S"><use xlink:href="#icon-S" /></svg>
		<div class="font-bold text-xl text-white ">K</div>
	</span>
</div>
