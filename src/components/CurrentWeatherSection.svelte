<script>
  // Mapping weather icons based on weather codes
  const weatherIcons = {
    "01d": "https://i.ibb.co/Sv7NfpZ/sunny-img.png",
    "01n": "https://i.ibb.co/SX5npVx/pngwing-com-10.png",
    "02d": "https://i.ibb.co/BKmxYdf/pngwing-com-1.png",
    "02n": "https://i.ibb.co/TPRrC9k/pngwing-com-2.png",
    "03d": "https://i.ibb.co/0CbzTL0/218706.png",
    "03n": "https://i.ibb.co/0CbzTL0/218706.png",
    "04d": "https://i.ibb.co/CMrdQ01/85502.png",
    "04n": "https://i.ibb.co/CMrdQ01/85502.png",
    "09d": "https://i.ibb.co/n7wn5xp/pngwing-com-6.png",
    "09n": "https://i.ibb.co/n7wn5xp/pngwing-com-6.png",
    "10d": "https://i.ibb.co/t3QXNDK/rain-with-sun-img.png",
    "10n": "https://i.ibb.co/LvmCyjt/pngwing-com-8.png",
    "11d": "https://i.ibb.co/y0qRTPZ/pngwing-com-4.png",
    "11n": "https://i.ibb.co/y0qRTPZ/pngwing-com-4.png",
    "13d": "https://i.ibb.co/8cBmNcs/pngwing-com-5.png",
    "13n": "https://i.ibb.co/8cBmNcs/pngwing-com-5.png",
    "15d": "https://i.ibb.co/rGPBFnd/pngwing-com-11.png",
    "15n": "https://i.ibb.co/rGPBFnd/pngwing-com-11.png"
  };

  export let currentWeather;
  export let currentCityName;
  export let unit = 'metric';
</script>

<div class="flex flex-col items-center bg-blue-100 border border-blue-200 border-3 rounded-md p-4 pt-2 mt-4 md:flex-row md:justify-between md:pb-5 md:pl-8 md:pr-8">
  <div class="flex flex-col items-center md:items-start">
    <h1 class="city-name text-lg font-bold text-center md:text-2xl md:mt-0 md:mb-2">Current Weather in {currentCityName}</h1>
    <p class="cur-temp hidden md:block text-5xl font-bold m-0">{Math.ceil(currentWeather.main.temp)}&deg;{unit === 'metric' ? 'C' : 'F'}</p>
    <p class="weather-desc text-base font-semibold capitalize text-gray-500 mt-2 md:text-lg">{currentWeather.weather[0].description}</p>
  </div>
  <div class="flex items-center justify-center mt-3 md:mt-0">
    <img src={weatherIcons[currentWeather.weather[0].icon]} alt="Weather Icon" class="w-24 h-24 md:w-40 md:h-40" />
    <p class="text-3xl font-medium ml-3 text-gray-700 md:hidden">{Math.ceil(currentWeather.main.temp)}&deg;{unit === 'metric' ? 'C' : 'F'}</p>
  </div>
</div>

