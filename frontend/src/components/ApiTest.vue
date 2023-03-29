<script>
export default {
  data: () => ({
    apiResponse: null,
    apiWeatherResponse: null
  }),

  created() {
    this.fetchData(),
    this.fetchWeatherData()
  },

  methods: {
    async fetchData() {
      const url = 'http://localhost/'
      this.apiResponse = await (await fetch(url)).json()
    },
    async fetchWeatherData(latitude, longitude){
      const url = 'http://localhost/web/$latitude/$longitude';
      this.apiWeatherResponse = await (await fetch(url)).json();
      console.log(this.apiWeatherResponse.current)
    },
    getData(item) {
      console.log(item.latitude);
      console.log(item.longitude);
      this.fetchWeatherData(item.latitude, item.longitude);
    },

  }
}
</script>

<template>
  <div v-if="!apiResponse">
    Pinging the api...
  </div>

  <div v-if="apiResponse">
    The api responded with: <br />
    <code>
          <div class="relative overflow-x-auto">
        <table class="w-full text-sm text-left text-gray-500 dark:text-gray-400">
            <thead class="text-xs text-gray-700 uppercase bg-gray-50 dark:bg-gray-700 dark:text-gray-400">
                <tr>
                    <th scope="col" class="px-6 py-3">
                        NAME
                    </th>
                    <th scope="col" class="px-6 py-3">
                        EMAIL
                    </th>
                    <th scope="col" class="px-6 py-3">
                        LATITUDE
                    </th>
                    <th scope="col" class="px-6 py-3">
                        LONGITUDE
                    </th>
                    <th scope="col" class="px-6 py-3">
                        
                    </th>    
                </tr>
            </thead>
            <tbody>
                <tr class="bg-white border-b dark:bg-gray-800 dark:border-gray-700">
                    <th scope="row" class="px-6 py-4 font-medium text-gray-900 whitespace-nowrap dark:text-white">
                      <li v-for="(item) in apiResponse.users">
                        {{ item.name }}
                      </li>
                    </th>
                    <td class="px-6 py-4">
                      <li v-for="(item) in apiResponse.users">
                        {{ item.email }}
                      </li> 
                    </td>
                    <td class="px-6 py-4">
                      <li v-for="(item) in apiResponse.users">
                        {{ item.latitude }}
                      </li> 
                    </td>
                    <td class="px-6 py-4">
                      <li v-for="(item) in apiResponse.users">
                        {{ item.longitude }}
                      </li> 
                    </td>
                    <td>
                      <li v-for="(item) in apiResponse.users">
                        <button v-on:click="getData(item)">GET DATA</button>
                      </li>
                    </td>
                </tr>
            </tbody>
        </table>
        <table class="w-full text-sm text-left text-gray-500 dark:text-gray-400">
            <thead class="text-xs text-gray-700 uppercase bg-gray-50 dark:bg-gray-700 dark:text-gray-400">
                <tr>
                    <th scope="col" class="px-6 py-3">
                        REGION
                    </th>
                    <th scope="col" class="px-6 py-3">
                        COUNTRY
                    </th>
                    <th scope="col" class="px-6 py-3">
                        TEMP CEL
                    </th>
                    <th scope="col" class="px-6 py-3">
                        TEMP FAR
                    </th>
                    <th scope="col" class="px-6 py-3">
                        HUMIDITY
                    </th>    
                </tr>
            </thead>
            <tbody>
                <tr class="bg-white border-b dark:bg-gray-800 dark:border-gray-700">
                    <th scope="row" class="px-6 py-4 font-medium text-gray-900 whitespace-nowrap dark:text-white">
                       {{ apiWeatherResponse.location.region }}
                    </th>
                    <td class="px-6 py-4">
                      {{ apiWeatherResponse.location.country }}
                    </td>
                    <td class="px-6 py-4">
                      {{ apiWeatherResponse.current.temp_c }}
                    </td>
                    <td class="px-6 py-4">
                      {{ apiWeatherResponse.current.temp_f }}
                    </td>
                    <td class="px-6 py-4">
                      {{ apiWeatherResponse.current.humidity }}
                    </td>
                </tr>
            </tbody>
        </table>
    </div>
        </code>
  </div>
</template>