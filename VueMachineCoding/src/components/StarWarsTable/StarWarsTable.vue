<template>
  <h1 v-if="isLoading">Loading...</h1>
  <table v-else border>
    <thead>
      <tr>
        <th>NAME</th>
        <th>VEHICLES</th>
        <th>FILMS</th>
      </tr>
    </thead>
    <tbody>
      <tr v-for="(value, index) in tableData" :key="value.name + index">
        <td>{{ value.name }}</td>
        <td>{{ value.vehicles }}</td>
        <td>{{ value.films }}</td>
      </tr>
    </tbody>
  </table>
</template>
<script setup>
import { onMounted, ref } from "vue";

const tableData = ref([]);
const isLoading = ref(false);

onMounted(() => {
  async function fetchData() {
    isLoading.value = true;
    try {
      let tempUserData = [];
      let response = await fetch("https://swapi.dev/api/people/?format=json");
      let userData = await response.json().then((res) => res.results);

      for (let user of userData) {
        let vehicles = await Promise.all(
          user.vehicles.map((vehicle) =>
            fetch(vehicle)
              .then((resp) => resp.json())
              .then((data) => data.name)
          )
        );

        let films = await Promise.all(
          user.films.map((film) =>
            fetch(film)
              .then((resp) => resp.json())
              .then((data) => data.title)
          )
        );

        tempUserData.push({
          name: user.name,
          vehicles: vehicles.join(","),
          films: films.join(","),
        });
      }

      tableData.value = tempUserData;
    } catch (error) {
      console.log(error);
    } finally {
      isLoading.value = false;
    }
  }

  fetchData();
});
</script>
<style scoped></style>
