<script>
	import { onMount, afterUpdate } from "svelte";
	import OITable from "../components/OITable.svelte";

	let symbolName = "NIFTY";
	let ceData = [];
	let peData = [];

	async function fetchData() {
		const response = await fetch("src/data/response.json");
		const data = await response.json();
		ceData = Object.values(data[symbolName].CE);
		peData = Object.values(data[symbolName].PE);
	}

	onMount(fetchData);
	afterUpdate(fetchData);
</script>

<h2>{symbolName} Tables:</h2>
<OITable {symbolName} {ceData} {peData} />
