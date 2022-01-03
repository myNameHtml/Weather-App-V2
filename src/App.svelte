<script>
	import DayAndLocation from "./components/DayAndLocation.svelte";
	import WeatherInfo from "./components/WeatherInfo.svelte";
	
	$: dalload = false;
	$: wiload = false;

	setTimeout(() => {
		wiload = true;
	}, 3500);

	setTimeout(() => {
		dalload = true;
	}, 3000);

</script>

<main>
	{#if dalload === true && wiload == true}
		<DayAndLocation/>
		<WeatherInfo/>
		{:else}
		<p class="text-center text-white display-3 logo-text mt-5">OpenWeatherApp</p>
		<img class="center-img mt-3 mb-5" src="https://cdn-icons.flaticon.com/png/512/2057/premium/2057945.png?token=exp=1641236169~hmac=749af9dc47ead8b73465e7084b88a1d0" alt="WIND ICON" width="200px" height="200px">
		<div class="center-div">
			<div class="spinner-border" style="width: 5rem; height: 5rem;" role="status">
				<span class="visually-hidden">Loading...</span>
			</div>
		</div>
	{/if}
	
</main>

<style>
	@import url('https://fonts.googleapis.com/css2?family=Ubuntu:wght@500&display=swap');
	.logo-text{
		font-weight: 600;
		font-family: 'Ubuntu', sans-serif;
	}

	.center-img{
		display: block;
  		margin-left: auto;
  		margin-right: auto;
	}
</style>