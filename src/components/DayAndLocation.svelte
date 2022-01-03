<script>
    import { onMount } from "svelte";
    import { fade } from "svelte/transition";
    import { GeoFiy_KEY } from "../../API_KEY/API.js";
    let time = new Date();
    const dateNameArr = ["Sunday", "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday"];
    const monthNames = ["January", "February", "March", "April", "May", "June", "July", "August", "September", "October", "November", "December"];
    let month = new Date().getMonth();
    let dayIndex = new Date().getDay();
    $: year = time.getFullYear();
    $: date = AddZeroBefore(new Date().getDate());
    $: hour = time.getHours();
    $: minute = time.getMinutes();
    $: hour = AddZeroBefore(hour);
    $: minute = AddZeroBefore(minute);
    $: city = "Loading...";
    $: progressBar = 0;

    function AddZeroBefore(time) {
        return (time < 10) ? ("0" + time) : time;
    }

    onMount(() =>{
        const inteval =  setInterval(() => {
            time = new Date();
        }, 5000);

        const Loading = setInterval(() => {
            progressBar += 1;
            if (progressBar === 100) {
                clearInterval(Loading);
            }
        }, 20);

        navigator.geolocation.getCurrentPosition(SuccessLocation, DeniedLocation, {
            enableHighAccuracy: true
        });
    })

    const SuccessLocation = (position) =>{
        // Instead of "GeoFiy_KEY" add own
        fetch(`https://api.geoapify.com/v1/geocode/reverse?lat=${position.coords.latitude}&lon=${position.coords.longitude}&apiKey=${GeoFiy_KEY}`, {method: "GET"})
        .then(response => response.json())
        .then(result => {
            city = result.features[0].properties.city;
        })
        .catch(error => console.log('error', error));
    }

    const DeniedLocation = (err) =>{
        console.log(err)
    }
</script>

<div class="d-block">
    <!--<div class="d-flex justify-content-center mt-1 text-white">{hour + ":" + minute}</div>-->
    <div transition:fade class="d-flex justify-content-center mt-5 display-3 text-white"><strong>{city}</strong></div>
    <div class="d-flex justify-content-center mt-3 lead text-white"><strong> {dateNameArr[dayIndex]+", "+date+"th "+monthNames[month]+" "+year}</strong></div>
</div>


<style>
</style>
