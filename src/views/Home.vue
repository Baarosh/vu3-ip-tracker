<template>
  <div class="flex flex-col h-screen max-h-screen">
    <!-- Search / Results -->
    <div class="flex justify-center relative bg-blue-600 px-4 pt-8 pb-32 ">
      <!-- Search Input -->
      <div class="w-full max-w-screen-sm">
        <h1 class="text-white text-center text-3xl pb-4">IP Tracker</h1>
        <div class="flex">
          <input
            type="text"
            class="flex-1 py-3 px-2 rounded-tl-md rounded-bl-md focus:outline-none"
            placeholder="Search for any IP or leave empty to get your IP info"
          />
          <i
            class="fas fa-chevron-right cursor-pointer bg-black text-white px-4 rounded-tr-md rounded-br-md flex items-center"
          ></i>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Home'
};
</script>
