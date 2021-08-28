<template>
  <div class="flex flex-col h-screen max-h-screen">
    <!-- Search / Results -->
    <div class="z-20 flex justify-center relative bg-blue-600 px-4 pt-8 pb-32">
      <!-- Search Input -->
      <div class="w-full max-w-screen-sm">
        <h1 class="text-white text-center text-3xl pb-4">IP Tracker</h1>
        <div class="flex">
          <input type="text" v-model="queryIp" class="flex-1 py-3 px-2 rounded-tl-md rounded-bl-md focus:outline-none text-xs" placeholder="Search for any IP or leave empty to get your IP info" />
          <i @click="getIpInfo" class="fas fa-chevron-right cursor-pointer bg-black text-white px-4 rounded-tr-md rounded-br-md flex items-center"></i>
        </div>
      </div>
      <IPInfo v-if="ipInfo" :ipInfo="ipInfo" />
    </div>
    <!-- Map -->
    <div id="mapid" class="h-full z-10"></div>
  </div>
</template>

<script>
import leaflet from 'leaflet';
import axios from 'axios';
import IPInfo from '../components/IPInfo.vue';

export default {
  name: 'Home',
  components: {
    IPInfo,
  },
  data() {
    return {
      queryIp: '',
      ipInfo: null,
      map: null,
    };
  },
  mounted() {
    this.map = leaflet.map('mapid');
    this.map.setView([51.1, 17.03], 9);

    leaflet
      .tileLayer('https://api.mapbox.com/styles/v1/{id}/tiles/{z}/{x}/{y}?access_token=pk.eyJ1IjoidHJlYnJvbnN6ZWYiLCJhIjoiY2tzdnE2N2d0MGJvaTJ3bHF4a3V4aGZ2bCJ9.E7zL-4hKYA_jGUgtWiXqog', {
        attribution: 'Map data &copy; <a href="https://www.openstreetmap.org/copyright">OpenStreetMap</a> contributors, Imagery © <a href="https://www.mapbox.com/">Mapbox</a>',
        maxZoom: 18,
        id: 'mapbox/streets-v11',
        tileSize: 512,
        zoomOffset: -1,
        accessToken: 'pk.eyJ1IjoidHJlYnJvbnN6ZWYiLCJhIjoiY2tzdnE2N2d0MGJvaTJ3bHF4a3V4aGZ2bCJ9.E7zL-4hKYA_jGUgtWiXqog',
      })
      .addTo(this.map);
  },
  methods: {
    async getIpInfo() {
      try {
        const data = await axios.get(`https://geo.ipify.org/api/v1?apiKey=at_LklPqN4Bpcuf4tRyqGtv3ml9uDEJB&ipAddress=${this.queryIp}`);
        const result = data.data;
        this.ipInfo = {
          address: result.ip,
          state: result.location.region,
          timezone: result.location.timezone,
          isp: result.isp,
          lat: result.location.lat,
          lng: result.location.lng,
        };
        leaflet.marker([this.ipInfo.lat, this.ipInfo.lng]).addTo(this.map);
        this.map.setView([this.ipInfo.lat, this.ipInfo.lng], 13);
      } catch (err) {
        // eslint-disable-next-line no-alert
        alert('Invalid IP address. Please correct!');
      }
    },
  },
};
</script>
