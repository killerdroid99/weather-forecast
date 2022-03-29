<script>
	import MapMarker from "svelte-material-icons/MapMarker.svelte";
	import Calender from "svelte-material-icons/CalendarToday.svelte";
	import High from "svelte-material-icons/ThermometerHigh.svelte";
	import Low from "svelte-material-icons/ThermometerLow.svelte";
	import Search from "svelte-material-icons/MapSearch.svelte";
	import HPercent from "svelte-material-icons/WaterPercent.svelte";
	import Prsr from "svelte-material-icons/CloudDownload.svelte";
	import WSpeed from "svelte-material-icons/WeatherWindy.svelte";
	import Card from "./Card.svelte";
	import Layout from "./Layout.svelte";
	$: temp = "";
	$: maxTemp = "";
	$: minTemp = "";
	$: pressure = "";
	$: humidity = "";
	$: windSpeed = "";
	$: status = "";
	let cityName = "";
	let country = "";
	let weatherIcon = "";

	const getBackgroundImage = async () => {
		const response = await fetch(
			`https://api.unsplash.com/photos/random?query=${cityName}&client_id=${process.env.UNSPLASH_API_KEY}`
		);
		const data = await response.json();
		return data.urls.regular;
	};

	const getWeather = async (apiKey, city, unit) => {
		try {
			let res = await fetch(
				`https://api.openweathermap.org/data/2.5/weather?q=${city}&appid=${apiKey}&units=${unit}`
			);
			let data = await res.json();
			unit === "metric"
				? (temp = `${data.main.temp} °C`)
				: (temp = `${data.main.temp} °F`);
			status = data.weather[0].description;
			country = data.sys.country;
			cityName = data.name;
			unit === "metric"
				? (maxTemp = `${data.main.temp_max} °C`)
				: (maxTemp = `${data.main.temp_max} °F`);
			unit === "metric"
				? (minTemp = `${data.main.temp_min} °C`)
				: (minTemp = `${data.main.temp_min} °F`);
			pressure = `${data.main.pressure} hPa`;
			humidity = `${data.main.humidity} %`;
			unit === "metric"
				? (windSpeed = `${data.wind.speed} m/s`)
				: (windSpeed = `${data.wind.speed} miles/hr`);
			weatherIcon = `https://openweathermap.org/img/wn/${data.weather[0].icon}.png`;
		} catch (error) {
			alert("City not found");
		}
	};
	$: fullLocation = `${cityName}, ${country}`;
	getWeather("31f87520d0472a687c7d90d468ec57ac", "london", "metric");

	let curLocation = "London";
	const handleclick = (e) => {
		curLocation = e.target.previousElementSibling.value;
		getWeather("31f87520d0472a687c7d90d468ec57ac", curLocation, "metric");
	};

	const handleSubmit = (e) => {
		curLocation = e.target.value;
		if (e.key === "Enter") {
			getWeather("31f87520d0472a687c7d90d468ec57ac", curLocation, "metric");
		}
	};

	const handleTemperature = (e) => {
		if (e.target.innerText === "°F") {
			getWeather("31f87520d0472a687c7d90d468ec57ac", curLocation, "imperial");
		} else {
			getWeather("31f87520d0472a687c7d90d468ec57ac", curLocation, "metric");
		}
	};
</script>

<Layout>
	<main>
		<h1>Weather App</h1>

		<input
			type="search"
			placeholder="Search here..."
			on:keypress={handleSubmit}
			value="london"
		/>
		<button on:click={handleclick}><Search width="25" height="22" /></button>
		<div class="toggleTemperatureUnit">
			<button on:click={(e) => handleTemperature(e)}>°F</button>
			<button on:click={(e) => handleTemperature(e)}>°C</button>
		</div>

		<Card>
			<h2><Calender />Today's Temperature : {temp}</h2>
			<span>
				<img src={weatherIcon} alt="icon" />
				<h4>{status}</h4>
			</span>
			<h3><MapMarker color="#00f5d4" /> Location : {fullLocation}</h3>
			<h3 class="max"><High />Maximum Temperature : {maxTemp}</h3>
			<h3 class="min"><Low />Minimum Temperature : {minTemp}</h3>
			<h3
				><Prsr color="#0077b6" height="25" width="20" />Pressure : {pressure}</h3
			>
			<h3
				><HPercent color="#264653" height="25" width="25" />Humidity : {humidity}</h3
			>
			<h3
				><WSpeed color="#ccc" height="25" width="25" />Wind Speed : {windSpeed}</h3
			>
		</Card>
	</main>
</Layout>

<style>
	main {
		text-align: center;
		padding: 1em;
		max-width: 240px;
		margin: 0 auto;
		color: aliceblue;
	}

	h2 {
		display: inline-flex;
		align-items: center;
		margin: 0 auto;
	}

	h3 {
		display: flex;
		align-items: center;
		gap: 0.25em;
		margin-inline: auto;
	}

	span {
		display: flex;
		margin: 1em auto;
		max-width: 20em;
		align-items: center;
		justify-content: center;
		background-image: linear-gradient(to left, #f8bc5b, #f18e31);
		border-radius: 1.5em;
		padding: 0.5em;
		z-index: 1;
	}

	h1 {
		color: #ff3e00;
		text-transform: uppercase;
		font-size: 4em;
		font-weight: 100;
		text-decoration: underline;
	}

	.max {
		color: #ff0000;
		text-transform: uppercase;
		font-weight: 600;
	}

	.min {
		color: #00b7ff;
		text-transform: uppercase;
		font-weight: 600;
	}

	input {
		padding: 0.5em;
		border: none;
		border-bottom: 5px solid #ff3e00;
		font-size: 1.2em;
		font-weight: 100;
		text-transform: uppercase;
	}

	input:focus {
		outline: none;
		filter: drop-shadow(0 0 0.5em #ff3e00);
	}

	button {
		background: #ff3e00;
		color: #fff;
		padding: 0.5em;
		border: none;
		border-radius: 3px;
		font-size: 1em;
		font-weight: 600;
		cursor: pointer;
	}

	h4 {
		color: rgb(78, 78, 78);
		text-transform: uppercase;
	}

	button {
		width: 50px;
		height: 50px;
	}

	@media (min-width: 640px) {
		main {
			max-width: none;
			font-size: 1em;
		}
	}
</style>
