<template>
	<div id="app">
		<main>
			<div class="weather-container">
				<div class="search-box">
					<input type="text" v-model="query" @keyup.enter="fetchWeather" class="search-bar" placeholder="Cidade" />
				</div>

				<div class="weather-main" v-if="typeof weather.main != 'undefined'">
					<div class="location-box">
						<div class="location">{{ cityName() }}, {{ countryName() }}</div>
						<div class="date">{{ date() }}</div>
					</div>

					<div class="weather-box">
						<div class="temp">{{ tempDisplay() }}</div>

						<div class="weather">{{ wheaterDisplay() }}</div>
					</div>
				</div>
			</div>
		</main>
	</div>
</template>

<script>
export default {
	name: "app",
	data() {
		return {
			api_key: "2f66048c0472a33ffb7149e006c911b5",
			query: "",
			weather: {},
		};
	},

	methods: {
		async fetchWeather() {
			const response = await fetch(`http://api.openweathermap.org/data/2.5/weather?q=${this.query}&lang=pt_br&units=metric&appid=${this.api_key}`);
			const data = await response.json();
			this.setResults(data);
		},

		setResults(results) {
			this.weather = results;
		},

		cityName() {
			return this.weather.name;
		},

		countryName() {
			return this.weather.sys.country;
		},

		date() {
			const locale = "pt-br";
			const option = {
				year: "numeric",
				month: "short",
				weekday: "long",
				day: "numeric",
			};
			const newDate = new Date().toLocaleString(locale, option);
			// Deixa a primeira letra como maiuscula
			const date = newDate.charAt(0).toUpperCase() + newDate.slice(1);
			return date;
		},

		tempDisplay() {
			const temp = Math.round(this.weather.main.temp);
			const metric = "°C";
			return `${temp}${metric}`;
		},

		wheaterDisplay() {
			const description = this.weather.weather[0].description;
			// Deixa a primeira letra como maiuscula
			const clima = description.charAt(0).toUpperCase() + description.slice(1);
			return clima;
		},
	},
};
</script>

<style>
@import url("https://fonts.googleapis.com/css2?family=Montserrat:ital,wght@0,100;0,200;0,300;0,400;0,500;0,600;0,700;0,800;0,900;1,100;1,200;1,300;1,400;1,500;1,600;1,700;1,800;1,900&display=swap");

* {
	box-sizing: border-box;
	margin: 0;
	padding: 0;
}

body {
	font-family: "Montserrat", sans-serif;
	color: rgb(48, 48, 48);
	background-image: url("./assets/warm-bg.jpg");
	background-size: cover;
	background-position: bottom;
	transition: 0.4s;
}

main {
	max-width: 800px;
	margin: auto;
}

.weather-container {
	min-height: 100vh;
	padding: 2em;
}

.weather-main {
	background-image: linear-gradient(to bottom, rgba(0, 0, 0, 0.25), rgba(0, 0, 0, 0.75));
	padding: 1em;
	border-radius: 0.3em;
	animation: slide-in-elliptic-top-fwd 0.7s cubic-bezier(0.250, 0.460, 0.450, 0.940) both;
}

.search-box {
	width: 100%;
	margin-bottom: 2em;
}

.search-box .search-bar {
	display: block;
	width: 100%;
	padding: 1em;
	background-color: rgba(255, 255, 255, 0.8);
	border-radius: 0.3em;
	appearance: none;
	outline: none;
	transition: 0.2s;
	text-transform: capitalize;
}

.search-box .search-bar:focus {
	background-color: rgb(255, 255, 255);
	box-shadow: 0em 0em 0.3em rgba(0, 0, 0, 0.2);
}

.location-box {
	text-align: center;
	color: #ffffff;
	margin-bottom: 1.5em;
}

.location-box .location {
	font-size: 32px;
	font-weight: bold;
	text-shadow: 1px 3px rgba(0, 0, 0, 0.25);
}

.location-box .date {
	font-size: 20px;
	font-weight: 200;
	font-style: italic;
}

.weather-box {
	text-align: center;
	color: #ffffff;
}

.weather-box .temp {
	font-size: 95px;
	font-weight: 800;
	text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
	background-color: rgba(255, 255, 255, 0.25);
	border-radius: 0.1em;
	box-shadow: 3px 6px rgba(0, 0, 0, 0.25);
	max-width: 3em;
	margin: auto;
}

.weather-box .weather {
	font-size: 30px;
	font-weight: 300;
	font-style: italic;
	margin: 0.7em 0 0.2em 0;
}

@keyframes slide-in-elliptic-top-fwd {
  0% {
    transform: translateY(-600px) rotateX(-30deg) scale(0);
    transform-origin: 50% 100%;
    opacity: 0;
  }
  100% {
    transform: translateY(0) rotateX(0) scale(1);
    transform-origin: 50% 1400px;
    opacity: 1;
  }
}

</style>
