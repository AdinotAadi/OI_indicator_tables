<script>
	let selectedSymbols = [];

	const symbolOptions = [
		{ name: "NIFTY", id: "symbol1" },
		{ name: "BANKNIFTY", id: "symbol2" },
		{ name: "FINNIFTY", id: "symbol3" },
		{ name: "MIDCPNIFTY", id: "symbol4" },
		{ name: "SENSEX", id: "symbol5" },
		{ name: "BANKEX", id: "symbol6" },
	];

	function generateUrl() {
		const selectedParams = selectedSymbols.map(
			(symbol) => `${symbol.id}=${symbol.name}`
		);
		const baseUrl =
			"https://website-backend-ashy.vercel.app/analytics/oi_indicator/";
		return baseUrl + "?" + selectedParams.join("&");
	}
</script>
