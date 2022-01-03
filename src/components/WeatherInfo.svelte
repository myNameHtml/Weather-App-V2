<script>
    import { onMount } from "svelte";
    import { OpenWeather_KEY } from "../../API_KEY/API.js";
    import { fade } from "svelte/transition";
    let dash = "-"
    $: temperatur = "Temp.";
    $: feelsLike = "Feels.";
    $: tempMax = "Max";
    $: tempMin = "Min";
    $: clouds = "Cloud";
    $: desc = "Weather desc.";
    $: icon = "10d";
    $: hours = new Date().getHours();
	$: isDayTime = hours >= 6 && hours <= 18;
    setInterval(() => {
            hours = new Date().getHours()
            isDayTime = hours >= 6 && hours <= 18
            if (isDayTime === false) {
                window.document.body.classList.toggle("bg-night");
            }else{
                window.document.body.classList.toggle("bg-day");
            }
    }, 60000);

    onMount(() =>{
        
        if (isDayTime === false) {
            window.document.body.classList.toggle("bg-night");
        }else if(isDayTime === true){
            window.document.body.classList.toggle("bg-day");
        }
        
        navigator.geolocation.getCurrentPosition(SuccessLocation, DeniedLocation, {
            enableHighAccuracy: true
        });
    });

    async function SuccessLocation (position){
        // Instead of "OpenWeather_KEY" add own
        const res = await fetch(`https://api.openweathermap.org/data/2.5/weather?lat=${position.coords.latitude}&lon=${position.coords.longitude}&appid=${OpenWeather_KEY}&units=metric`)
        const result = await res.json();
        temperatur = Math.floor(result.main.temp).toString()+"°c";
        feelsLike = result.main.feels_like;
        tempMax = result.main.temp_max
        tempMin = result.main.temp_min
        clouds = result.clouds.all;
        desc = result.weather[0].description.toUpperCase();
        icon = result.weather[0].icon;
    }

    const DeniedLocation = (err) =>{
        console.log(err);
    }
</script>
<p transition:fade class="display-1 d-flex justify-content-center text-white mt-2 mb-0">{temperatur}</p>
<p class="d-flex justify-content-center text-white dash mb-3">{dash.repeat(temperatur.length*4.5)}</p>
<p class="d-flex justify-content-center text-white display-3 mb-5">{desc}</p>

<div class="container">
    <div class="row">
        <div class="col">
            <div class="text-center display-6 text-white info-card">
                Feels Like
                <p class="mt-2 display-5">{feelsLike}°c</p>
            </div>
        </div>
        
        <div class="col">
            <div class="text-center display-6 text-white info-card">
                Temp Max.
                <p class="mt-2 display-5">{tempMax}°c</p>
            </div>
        </div>
        
        <div class="col text-white">
            <div class="text-center text-white info-card">
                <img src="https://openweathermap.org/img/wn/{icon}@4x.png" width="140px" height="150px" alt="Weather Icon">
            </div>
        </div>
        
        <div class="col">
            <div class="text-center display-6 text-white info-card">
                Temp Min.
                <p class="mt-2 display-5">{tempMin}°c</p>
            </div>
        </div>

        <div class="col">
            <div class="text-center display-6 text-white info-card">
                Clouds
                <p class="mt-2 display-5">{clouds}%</p>
            </div>
        </div>
    </div>
</div>
<style>
    @import url('https://fonts.googleapis.com/css2?family=Inconsolata:wght@700&display=swap');
    :global(body){
        min-height:100%;
        background: linear-gradient(315deg, rgba(178,255,255,1) 0%, rgba(0,127,255,1) 100%);
        background-size:cover;
    }
    
    :global(body.bg-night){
        background:linear-gradient(0deg, rgba(12, 4, 0, 0.3), rgba(87, 87, 87, 0.3));
        background-image: url("https://images6.alphacoders.com/681/thumb-1920-681134.jpg");
        background-repeat: no-repeat;
        background-size: cover;
    }

    :global(body.bg-day){
        background:linear-gradient(0deg, rgba(12, 4, 0, 0.3), rgba(87, 87, 87, 0.3));
        background-image: url("https://images8.alphacoders.com/679/thumb-1920-679478.jpg");
        background-size: cover;
    }

    .display-1{
        font-size: calc(5.625rem + 4.5vw);
        font-weight: 300;
        line-height: 1.2;
        font-family: 'Inconsolata', monospace;
    }

    .dash{
        font-size: 2em;
        font-weight: 300;
        font-family: 'Inconsolata', monospace;
    }

    .info-card{
        background-color: rgba(0, 0, 0, 0.25);
        border-radius: 0.8rem;
        transition: 200ms
    }

    .info-card:hover{
        background-color: rgba(0, 0, 0, 0.35);
    }

    
</style>


