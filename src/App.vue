<template>
  <div id="app">
    <header>
      <img class="logo" src="./assets/logo.png" alt="">
      <h1 class="app-title">Custom Cars Garage</h1>
    </header>
    <AddCar
        @addCar="onAddCar"
        v-bind:engines="engineTypes"
        v-bind:makerAndModels="makerAndModelTypes"
    />
    <hr>
    <div class="list-wrap">
    <div class="filter-wrap ">
      <span class="engine-type-label">Engine Type Filter : </span>
      <EngineTypeDropdown
          @onSelectedEngine="onEngineSelectFilter"
          v-bind:engines="engineTypes"
       />
      </div>
      <CarsList 
        v-bind:cars="cars"
      />
    </div>  
    <footer>
      <img class="footer-img" src="./assets/carbackground.jpg" alt="">
      <div class="footer-title-wrap">
        <h2 class="footer-title">Custom Cars Garage</h2>
      </div>
    </footer>
  </div>
</template>

<script>
import AddCar from './components/AddCar.vue';
import CarsList from './components/CarsList.vue';
import EngineTypeDropdown from './components/EngineTypeDropdown.vue';
// database
import carsListJson from './assets/cars-list.json';
import engineTypesJson from './assets/engineTypes.json';
import modelsJson from './assets/models.json';

const engineTypes = engineTypesJson;
const makerAndModelTypes = modelsJson;
let userInputCars = JSON.parse(localStorage.getItem("cars")) || [];

export default {
    created: function() {
    if (userInputCars.length > 0) {
      carsListJson.concat(userInputCars);
      this.cars = [...carsListJson, ...userInputCars]
    }
  },
  data: () => {
    return {
      engineTypes,
      makerAndModelTypes,
      cars: carsListJson,
      userInputCars,
    }
  },
  name: 'App',
  components: {
    AddCar,
    CarsList,
    EngineTypeDropdown
  },
  methods: {
    onAddCar: function(car) {
      // save car to local storage
      this.userInputCars.push(car)
      localStorage.setItem("cars", JSON.stringify(userInputCars));
      this.cars.push(car);
    },
    onEngineSelectFilter(filterEngine) {
      
      if (filterEngine.engine === 'all') {
        this.cars = [...carsListJson, ...userInputCars];
      } else {
        this.cars = [
          ...carsListJson.filter(car => car.engine === filterEngine.engine.name),
          ...userInputCars.filter(car => car.engine === filterEngine.engine.name)
        ];
      }
      
    }
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  text-align: center;
  color: #2c3e50;
  position: relative;
  min-height: 100vh;
}

header {
  background-color: rgb(189, 189, 189);
  display: flex;
  justify-content: flex-start;
  align-items: center;
}
.logo {
  height: 50px;
}
.app-title {
  display: inline;
}

footer {
  height: 15vh;
  width: 100%;
  position: relative;
  bottom: 0;
}
.footer-img {
  width: 100%;
  max-height: 100%;
}

.footer-title-wrap {
  width: 95%;
  height: 90%;
  background-color: #c0c0c0;
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  display: flex;
  justify-content: center;
  align-items: center;
  opacity: 0.75;
}

.engine-type-label{
  margin-right: 40px;
}

.filter-wrap {
  background-color: #c0c0c0;
  height: 50px;
  display: flex;
  align-items: center;
  justify-content: flex-end;
  margin: 0 50px 0 50px;
}

.filter-wrap select {
  margin-right: 8px;
  width: initial;
}
.list-wrap {
  display: flex;
  flex-direction: column;
  height: 60vh;
}

</style>
