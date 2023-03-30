<script>
export default {
  data: () => ({
    apiResponse: null,
    apiWeatherResponse: null,
  }),

  created() {
    this.fetchData();
  },

  methods: {
    async fetchData() {
      const url = "http://localhost/";
      this.apiResponse = await (await fetch(url)).json();
    },
    async fetchWeatherData(latitude, longitude) {
      console.log(latitude);
      console.log(longitude);
      const url = `http://localhost/web/${latitude}/${longitude}`;
      console.log(url);
      this.apiWeatherResponse = await (await fetch(url)).json();
      console.log(this.apiWeatherResponse.current);
    },
    getData(item) {

      return this.fetchWeatherData(item.latitude.slice(0,-4), item.longitude.slice(0,-4));

    },
  },
};
</script>

<template>
  <div v-if="!apiResponse">Pinging the api...</div>

  <div v-if="apiResponse">
    The api responded with: <br />
    <code>
        <div class="relative overflow-x-auto">
          <table
            class="w-full text-sm text-left text-gray-500 dark:text-gray-400"
          >
            <thead
              class="text-xs text-gray-700 uppercase bg-gray-50 dark:bg-gray-700 dark:text-gray-400"
            >
              <tr>
                <th scope="col" class="px-6 py-3">NAME</th>
                <th scope="col" class="px-6 py-3">EMAIL</th>
                <th scope="col" class="px-6 py-3">LATITUDE</th>
                <th scope="col" class="px-6 py-3">LONGITUDE</th>
                <th scope="col" class="px-6 py-3"></th>
              </tr>
            </thead>
            <tbody>
              <tr
                v-for="item in apiResponse.users"
                class="bg-white border-b dark:bg-gray-800 dark:border-gray-700"
              >
                <th
                  scope="row"
                  class="px-6 py-4 font-medium text-gray-900 whitespace-nowrap dark:text-white"
                >
                  <li>
                    {{ item.name }}
                  </li>
                </th>
                <td class="px-6 py-4">
                  <li>
                    {{ item.email }}
                  </li>
                </td>
                <td class="px-6 py-4">
                  <li>
                    {{ item.latitude }}
                  </li>
                </td>
                <td class="px-6 py-4">
                  <li>
                    {{ item.longitude }}
                  </li>
                </td>
                <td>
                  <li>
                    <button v-on:click="getData(item)">GET DATA</button>
                  </li>
                </td>
              </tr>
            </tbody>
          </table>
          <table
            class="w-full text-sm text-left text-gray-500 dark:text-gray-400"
          >
            <thead
              class="text-xs text-gray-700 uppercase bg-gray-50 dark:bg-gray-700 dark:text-gray-400"
            >
              <tr>
                <th scope="col" class="px-6 py-3">COUNTRY</th>
                <th scope="col" class="px-6 py-3">TEMP CEL</th>
                <th scope="col" class="px-6 py-3">TEMP FAR</th>
                <th scope="col" class="px-6 py-3">HUMIDITY</th>
              </tr>
            </thead>
          
            <tbody v-if="apiWeatherResponse">
              <tr class="bg-white border-b dark:bg-gray-800 dark:border-gray-700">
                <th
                  scope="row"
                  class="px-6 py-4 font-medium text-gray-900 whitespace-nowrap dark:text-white"
                >
                {{ apiWeatherResponse.location.country }}
                </th>
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
