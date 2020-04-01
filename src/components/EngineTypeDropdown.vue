<template>
  <div class="engine-type-dropdown-wrap">
            <select class="select-engine" v-model="selectedEngine" v-on:change="onSelectedEngine">
                <option disabled value="">Engine Type</option>
                <option value="all">All</option>
                <option v-for="(engine, index) in engines" :key="index" :value="engine">{{engine.name}}</option>
            </select>
  </div>
</template>

<script>
export default {
    name: 'EngineTypeDropdown',
    created: function() {
        if (this.$parent.addCar) {
            this.selectedEngine = '';
        } else {
            this.selectedEngine = JSON.parse(localStorage.getItem("engineFilter")) || '';
            this.$emit('onSelectedEngine',{
                engine: this.selectedEngine
            });
        }
    },
    data: function() {
        return {
            selectedEngine: '',
        }
    },
    methods: {
        onSelectedEngine: function() {
            localStorage.setItem("engineFilter", JSON.stringify(this.selectedEngine));
            this.$emit('onSelectedEngine',{
                engine: this.selectedEngine
            });
        },
        },
    props: {
        engines:{
            type: Array
        },
    },

}
</script>

<style>
.engine-type-dropdown-wrap {
    display: inline;
}
.select-engine {
    width: 300px;
}

select {
    border: 0;
    border-bottom: 2px solid #0a68a9;
    background-color: #ffffff;
}
select:focus {
    outline: none;
}

</style>