<script lang="ts">
    import { onMount } from "svelte";

    let weather: { properties: { periods: any; }; } | undefined;

    onMount(async () =>
    {
        weather = await getWeather();
        console.log(weather);
    });

    const getWeather = async () => 
    {
        let response = await fetch("https://api.weather.gov/points/42.7284,-73.6917");
        const initialData = await response.json();
        console.log(initialData);

        response = await fetch(initialData.properties.forecast);
        const data = await response.json();

        return data;
    }
</script>

<h1>Weather Display</h1>
<p>This page displays weather data for Troy, NY using the National Weather Service API.</p>

<table role="grid">
    <thead>
        <th scope="col">Day</th>
        <th scope="col">Temperature</th>
        <th scope="col">Humidity</th>
        <th scope="col">Rainfall</th>
    </thead>
    <tbody>
        {#if weather !== undefined}
        {#each weather.properties.periods as row}
            <tr> 
                <td>{row.name} </td>
                <td>{row.temperature}</td>
                <td>{row.relativeHumidity.value}</td>
                <td>{row.probabilityOfPrecipitation.value}</td>
            </tr>
        {/each}
        {/if}
        <th scope="row"></th>
    </tbody>
</table>