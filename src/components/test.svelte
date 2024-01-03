<script>
	import { onMount } from "svelte";

	let niftyChecked = false;
	let bankNiftyChecked = false;
	let result = "";

	const fetchStockData = async () => {
		let apiUrl =
			"https://website-backend-ashy.vercel.app/analytics/oi_indicator/?";

		if (niftyChecked) {
			apiUrl += "symbol1=NIFTY&";
		}

		if (bankNiftyChecked) {
			apiUrl += "symbol2=BANKNIFTY&";
		}

		// Remove the last '&' if present
		apiUrl = apiUrl.replace(/&$/, "");

		try {
			const response = await fetch(apiUrl);
			const data = await response.json();
			result = JSON.stringify(data, null, 2);
		} catch (error) {
			console.error("Error fetching data:", error);
		}
	};

	onMount(() => {
		// Fetch data on component mount (optional)
		fetchStockData();
	});
</script>

<main>
	<h1>Stock Data Fetcher</h1>
	<label>
		<input type="checkbox" bind:checked={niftyChecked} /> Nifty
	</label>
	<label>
		<input type="checkbox" bind:checked={bankNiftyChecked} /> BankNifty
	</label>
	<br />
	<button on:click={fetchStockData}>Fetch Data</button>
	<div>{result}</div>
</main>

<style>
  body {
    font-family: Arial, sans-serif;
    text-align: center;
    margin: 20px;
  }

  label {
    margin-right: 10px;
  }

  button {
    padding: 10px;
    margin-top: 10px;
    cursor: pointer;
  }

  div {
    margin-top: 20px;
  }
</style>
