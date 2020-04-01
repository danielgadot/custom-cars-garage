<template>
  <div class="cars-list-wrap">
    <table class="cars-list-table">
        <thead>
            <tr>
                <th>PlateNumber</th>
                <th>Make & model</th>
                <th>EngineType</th>
            </tr>
        </thead>
        <tbody>
            <tr v-for="(car, index) in cars" :key="index">
                <td>{{car.plate}}</td>
                <td>{{car.makerAndModel}}</td>
                <td>{{car.engine}}</td>
            </tr>
        </tbody>
    </table>
  </div>
</template>

<script>
export default {
  name: 'CarsList',
  props: {
    msg: String,
    cars: {
      type: Array
    }
  }
}
</script>

<style scoped>

.cars-list-table {
  width: 100%;
  border-collapse: collapse;
}
th, td {
  width: 33.33%; /* to enable "word-break: break-all" */
  padding: 5px;
  word-break: break-all; /* 4. */
}

thead { 
  background-color: #7d7d7d;
  color: #ffffff;
  display: table;
  width: 100%;
}
tr {
  height: 32px;
  border: 1px solid #7d7d7d40;
  display: table;
  width: 100%;
  word-break: break-all;
}

.cars-list-wrap {
  margin: 0 50px 0 50px;
}
tbody {
  display: block;
  max-height: 360px;
  overflow-y: scroll;
  width: 100%;
  overflow-x: hidden;
}
</style>
