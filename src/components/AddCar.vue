<template>
  <div class="add-car-wrap">
    <div class="add-car-form">

        <div class="field-wrap">
            <span class="form-label">Engine Type:</span>
                <EngineTypeDropdown
                  @onSelectedEngine="onEngineSelectNewCar"
                  v-bind:engines="engines"
                />
        </div>
        <div class="field-wrap">

            <span class="form-label">Make & Model:</span>
            <select v-model="selectedMakerAndModel" class="form-value">
              <option disabled value="">Please select Maker and Model</option>
              <option v-for="(makerAndModel, index) in makerAndModelsFiltered" :key="index" :value="makerAndModel">{{makerAndModel.name}}</option>
            </select>
        </div>
        <div class="field-wrap">

            <span class="form-label">Plate Number:</span>
            <input v-model="plateNumber" type="number" :class="{invalid: !validPlate}"    @keypress="validatePlateNumber()" class="form-value plate-number-inp">
        </div>
        <div class="field-wrap">
            <button 
              :disabled="this.plateNumber < 1000 || !this.selectedEngine || !this.selectedMakerAndModel" 
              class="add-car-btn" 
              v-on:click="addCar"
            >Add Car</button>
        </div>
    </div>
    </div>
</template>

<script>
import EngineTypeDropdown from './EngineTypeDropdown.vue';

export default {
  data: () => {
    return {
      plateNumber: 0,
      selectedMakerAndModel: '',
      selectedEngine: '',
      makerAndModelsFiltered: [],
      validPlate: true
    }
  },
  methods: {
      addCar: function () {
        this.$emit('addCar', {
          plate: this.plateNumber,
          makerAndModel: this.selectedMakerAndModel.name,
          engine: this.selectedEngine.name,

        })
      },
      validatePlateNumber() {
        if (this.plateNumber < 1000) {
          this.validPlate = false;
        } else {
          this.validPlate = true;
        }
      },
      onEngineSelectNewCar(newCarEngine) {
        this.selectedEngine = newCarEngine.engine;
        this.makerAndModelsFiltered = this.makerAndModels.filter(makeAndModel => makeAndModel.engineType === this.selectedEngine.id)
      },
      filterMakerAndModels() {
        return this.makerAndModelsFiltered;
      }
  },
  name: 'AddCar',

  props: {
    engines:{
      type: Array
    },
    makerAndModels: {
      type: Array
    }
  },
    components: {
      EngineTypeDropdown
  },
}
</script>


<style scoped>
.field-wrap {
  margin: 12px 50px 12px 50px;
  align-items: start;
  display: flex;
}
.add-car-wrap {
  display: flex;
  align-items: flex-start;
  flex-direction: column;
}

.add-car-btn {
    background-color: transparent;
    color: #0a68a9;
    border: 2px solid #0a68a9;
    padding: 3px 100px 3px 100px;
    border-radius: 4px;
}
.add-car-btn:disabled{
  cursor: not-allowed;
}

.add-car-form {
  width: 50%;
}

.form-label {
  min-width: 400px;
  text-align: start;
}
.form-value {
  min-width: 300px;
}

.plate-number-inp {
  border: 0;
  border-bottom: 2px solid #0a68a9;
}
.plate-number-inp:focus {
  outline: none;
}

.invalid {
  color: red;
}
</style>
