<script>
	import { onMount, afterUpdate, onDestroy } from "svelte";
	import OITable from "../components/OITable.svelte";

	let apiUrl = "";
	let tablesData = [];

	const symbolOptions = [
		"NIFTY",
		"BANKNIFTY",
		"FINNIFTY",
		"MIDCPNIFTY",
		"SENSEX",
		"BANKEX",
	];
	let checkboxStates = {};
	symbolOptions.forEach((symbol) => {
		checkboxStates[symbol] = symbol === "NIFTY";
	});

	let selectedSymbols = ["NIFTY"];

	function constructApiUrl() {
		apiUrl = `https://website-backend-ashy.vercel.app/analytics/oi_indicator/?`;
		selectedSymbols.forEach((symbol, index) => {
			apiUrl += `symbol${index + 1}=${symbol}&`;
		});
		console.log(apiUrl);
	}

	onMount(() => {
		constructApiUrl();
		fetchData();
		startPolling();
	});

	onDestroy(() => {
		clearInterval(intervalId);
		checkboxStates = {};
	});

	let intervalId;

	function startPolling() {
		intervalId = setInterval(() => {
			constructApiUrl();
			fetchData();
		}, 10000); // 10000 milliseconds = 10 seconds
	}

	function handleCheckboxChange(event, symbol) {
		checkboxStates[symbol] = event.target.checked;
		selectedSymbols = symbolOptions.filter((symbol) => checkboxStates[symbol]);
		constructApiUrl();
	}

	async function fetchData() {
		const response = await fetch(apiUrl);
		const data = await response.json();

		tablesData = selectedSymbols.map((symbol) => {
			const ceData = data[symbol]?.CE || [];
			const peData = data[symbol]?.PE || [];

			return {
				symbolName: symbol,
				ceData: Object.values(ceData),
				peData: Object.values(peData),
			};
		});
	}
</script>

<div>
	{#each symbolOptions as symbol (symbol)}
		<label>
			<input
				type="checkbox"
				bind:checked={checkboxStates[symbol]}
				on:change={(e) => handleCheckboxChange(e, symbol)}
			/>
			{symbol}
		</label>
	{/each}
</div>

{#each tablesData as { symbolName, ceData, peData }}
	<h2>{symbolName} Tables:</h2>
	<OITable {symbolName} {ceData} {peData} />
{/each}
