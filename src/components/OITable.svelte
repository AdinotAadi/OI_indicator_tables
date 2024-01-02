<script>
	export let symbolName;
	export let ceData;
	export let peData;

	function calculateTotal(data) {
		let total = {
			Strike: "Total",
			"3 min": 0,
			"6 min": 0,
			"9 min": 0,
			"15 min": 0,
			"30 min": 0,
			"60 min": 0,
			"For Day": 0,
			"Total OI": 0,
		};

		Object.keys(total).forEach((column) => {
			if (column !== "Strike") {
				total[column] = data.reduce(
					(total, row) => total + parseFloat(row[column] || 0),
					0
				);
				total[column] = parseFloat(total[column]).toFixed(2);
			}
		});

		return total;
	}

	$: ceTotal = calculateTotal(ceData);
	$: peTotal = calculateTotal(peData);
</script>

<div class="table-container">
	<table>
		<tr>
			{#if ceData.length > 0}
				{#each Object.keys(ceData[0]) as column (column)}
					<th>{column}</th>
				{/each}
			{/if}
		</tr>

		{#each Object.keys(ceData) as key (key)}
			<tr>
				{#each Object.keys(ceData[key]) as column (column)}
					<td>{ceData[key][column]}</td>
				{/each}
			</tr>
		{/each}

		<tr>
			<td>{ceTotal.Strike}</td>
			{#each Object.keys(ceTotal).slice(1) as key (key)}
				<td>{parseFloat(ceTotal[key]).toFixed(2)}</td>
			{/each}
		</tr>
	</table>
	<table>
		<tr>
			{#if peData.length > 0}
				{#each Object.keys(peData[0]) as column (column)}
					<th>{column}</th>
				{/each}
			{/if}
		</tr>

		{#each Object.keys(peData) as key (key)}
			<tr>
				{#each Object.keys(peData[key]) as column (column)}
					<td>{peData[key][column]}</td>
				{/each}
			</tr>
		{/each}
		<tr>
			<td>{peTotal.Strike}</td>
			{#each Object.keys(peTotal).slice(1) as key (key)}
				<td>{parseFloat(peTotal[key]).toFixed(2)}</td>
			{/each}
		</tr>
	</table>
</div>

<style>
	h1 {
		text-align: center;
		margin-bottom: 20px;
	}

	h2 {
		margin-top: 30px;
		margin-bottom: 10px;
	}

	.table-container {
		margin: 0 auto;
		width: 95%;
		display: flex;
		flex-direction: row;
		gap: 20px;
		justify-content: space-between;
	}

	table {
		width: 48%;
		border-collapse: collapse;
		margin-bottom: 20px;
	}

	th,
	td {
		border: 1px solid #ddd;
		padding: 8px;
		text-align: left;
	}
</style>
