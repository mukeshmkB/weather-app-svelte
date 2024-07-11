<script>
  import { onMount } from 'svelte';
  import { Circle } from 'svelte-loading-spinners';

  import CurrentWeatherSection from "./CurrentWeatherSection.svelte";
  import ForeCastSection from "./ForeCastSection.svelte";

  // Define API status constants
  const apiStatusConstants = {
    initial: 'INITIAL',
    success: 'SUCCESS',
    failure: 'FAILURE',
    inProgress: 'IN_PROGRESS',
  };

  let apiStatus = apiStatusConstants.initial;
  let cityName = "New York";
  let unit = 'metric';
  let currentWeather = null;
  let currentCityName = '';
  let forecast = null;
  let searchHistory = [cityName];

  // Fetch weather data function
  const getWeatherData = async () => {
    apiStatus = apiStatusConstants.inProgress;

    const apiKey = 'b9204471b2c3b1127d17c57d322527bc';
    const apiUrl = `https://api.openweathermap.org/data/2.5/forecast?q=${cityName}&appid=${apiKey}&units=${unit}`;
    const options = {
      method: 'GET',
      headers: {
        Accept: 'application/json',
      },
    };

    try {
      const response = await fetch(apiUrl, options);
      if (response.ok) {
        const fetchedData = await response.json();
        apiStatus = apiStatusConstants.success;
        currentCityName = fetchedData.city.name;
        currentWeather = fetchedData.list[0];
        forecast = fetchedData.list;
      } else {
        throw new Error('Failed to fetch data');
      }
    } catch (error) {
      console.error('Error fetching weather data:', error.message);
      apiStatus = apiStatusConstants.failure;
    }
  };

  // Fetch weather data on component mount
  onMount(() => {
    getWeatherData();
  });

  // Toggle unit between metric and imperial
  const toggleUnit = async () => {
    unit = unit === 'metric' ? 'imperial' : 'metric';
    await getWeatherData();
  };

  // Filter forecast data by first object time
  const filteredForecastByFirstObjTime = (forecastDataList) => {
    if (!forecastDataList) {
      return [];
    }
    const firstObjTime = forecastDataList[0].dt_txt.split(' ')[1];
    const filteredObj = forecastDataList.filter(data => data.dt_txt.includes(firstObjTime));
    return filteredObj;
  };

</script>

<div class="bg-container flex bg-[#e7f2fc] p-5 border border-orangered min-h-screen md:pt-10">
  <div class="dashboard bg-white p-4 border-[3px] border-[#cececf] rounded-md flex-1 md:max-w-[950px] md:p-[35px] md:pt-[20px] md:mx-auto">
    <div class="flex justify-between items-center mb-4">
      <h1 class="text-[18px] font-[650] text-[#292828] m-0 md:text-3xl">Weather Dashboard</h1>
      <div class="switch w-[70px] h-[35px] flex items-center bg-[#a3cbf2] rounded-[6px] md:w-[110px] md:p-[10px]">
        {#if unit === 'metric'}
          <div class="switch-celsius flex justify-between items-center text-white ml-2" on:click={toggleUnit}>
            <div class="switch-toggle w-8 h-6 bg-white rounded md:w-16"></div>
            <p class="ml-2">C</p>
          </div>
        {:else}
          <div class="switch-fahrenheit flex justify-between items-center text-white ml-2" on:click={toggleUnit}>
            <p class="mr-2">F</p>
            <div class="switch-toggle w-8 h-6 bg-white rounded md:w-16"></div>
          </div>
        {/if}
      </div>
    </div>

    <form class="flex flex-col md:flex-row md:justify-between md:mt-5 md:mb-0" on:submit|preventDefault={getWeatherData}>
      <input type="search" placeholder="Enter the city name..." bind:value={cityName} class="bg-gray-200 text-sm py-2 pl-3 pr-2 rounded-md outline-none mb-4 md:w-[950px] md:p-4 md:pl-6 md:pr-4 md:mr-6 md:rounded-3xl" />
      <button class="bg-green-500 text-white text-sm w-20 h-7 rounded-md md:w-28 md:h-11 md:text-[15px] md:font-semibold md:rounded-3xl" type="submit">
        Search
      </button>
    </form>

    {#if apiStatus === apiStatusConstants.success}
      <div>
        <CurrentWeatherSection currentCityName={currentCityName} currentWeather={currentWeather} unit={unit} />
        <ForeCastSection filteredForeCast={filteredForecastByFirstObjTime(forecast)} unit={unit} />
      </div>
    {:else if apiStatus === apiStatusConstants.failure}
      <div class="failure-container flex flex-col justify-center items-center">
        <img src="https://i.ibb.co/GswLjyh/data-search-found-illustration-concept-108061-574.jpg" alt="request failed" class="w-72 md:w-96"/>
        <h1 class = "text-lg mt-0 md:xl" >Request Failed</h1>
        <p class = "text-sm font-medium mt-2 md:text:lg">Ensure the city name</p>
      </div>
    {:else if apiStatus === apiStatusConstants.inProgress}
      <div class="weather-loader-container flex justify-center items-center mt-10">
        <Circle color="#0b69ff" size="50" duration="1" />
      </div>
    {/if}
  </div>
</div>
