<script>
  export let filteredForeCast = [];
  export let unit = 'metric';

  // Function to get day from date string
  const getDay = data => {
    const date = new Date(data);
    return date.toLocaleDateString("en-IN", { weekday: "short" });
  }
</script>

<div class="mt-6">
  <h1 class="text-[18px] text-[#322f2f] font-bold md:text-2xl">5-Day Forecast</h1>
  <ul class="forecast-items flex flex-col items-center flex-wrap list-none mt-5 pl-0 md:flex-row">
    {#each filteredForeCast as eachItem}
      <li class="forecast-card flex flex-col justify-around w-[200px] p-[15px] bg-[#fcfcfc] border-[3px] border-[#eceaea] rounded-lg mb-5 md:max-w-[150px] md:mr-5" key={eachItem.dt_txt}>
        <h2 class="text-[16px] font-bold text-gray-800 mb-0">{getDay(eachItem.dt_txt)}</h2>
        <img
          src={`https://openweathermap.org/img/wn/${eachItem.weather[0].icon}@2x.png`}
          alt="Weather Icon"
          class="w-[110px] mx-auto"
        />
        <div class="self-end">
          <p class="text-[15px] font-semibold text-gray-800 mt-0 mb-1">High: {Math.ceil(eachItem.main.temp_max)}&deg;{unit === 'metric' ? 'C' : 'F'}</p>
          <p class="text-[15px] font-semibold text-gray-600 mt-0 mb-1">Low: {Math.ceil(eachItem.main.temp_min)}&deg;{unit === 'metric' ? 'C' : 'F'}</p>
        </div>
      </li>
    {/each}
  </ul>
</div>
