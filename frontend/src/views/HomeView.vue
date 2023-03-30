<template>
  <img class="background-image" src="polygons-bg.png">
  <v-card v-if="dataReady" class="main-card">
    <input title="Grid size" type="number" class="input" required
      minlength="1" maxlength="2" size="10" v-model="gridSize">
    <input title="occupied ratio" type="number" class="input" required
      minlength="1" maxlength="3" size="10" v-model="occupiedRatio">
    <input title="blocked ratio" type="number" class="input" required
      minlength="1" maxlength="3" size="10" v-model="blockedRatio">
      <button title="generate" class="generate-button" @click.prevent="onGenerateCoordinates">generate</button>
      <button title="generate" class="play-button" @click.prevent="onPlay">play</button>
      <div v-if="showGrid">
        <div v-for="row in tempGridSize" :key="row" class="row">
          <div v-for="col in tempGridSize" :key="col" class="square" :style="{ backgroundColor: coordinates[(row-1) * tempGridSize + col - 1].color, width: size + 'px', height: size + 'px' }">{{ (row-1) * tempGridSize + col }}</div>
        </div>
      </div>
   </v-card>
   <div v-if="!dataReady" class="loading-leaderboard-orbit-div">
      <orbit-spinner class="spinner"  :animation-duration="800" :size="150" color="#aa4646"/>
    </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue'
import { OrbitSpinner } from 'epic-spinners'
import ISquare from '../models/ISquare'
import 'vue3-toastify/dist/index.css'

export default defineComponent({
  name: 'HomeView',
  data () {
    return {
      bgimg: '../public/polygons-bg.png',
      dataReady: false,
      showGrid: false,
      coordinates: [] as ISquare[],
      size: 30,
      gridSize: 2,
      tempGridSize: 0,
      index: 1,
      blockedRatio: 0.25,
      occupiedRatio: 0.25
    }
  },
  components: {
    OrbitSpinner
  },
  methods: {
    onGenerateCoordinates () {
      this.coordinates = []
      const randomOccupied = this.getRandomValues(1, this.gridSize ** 2, Math.round(this.gridSize ** 2 * this.occupiedRatio))
      const randomBlocked = this.getRandomValues(1, this.gridSize ** 2, Math.round(this.gridSize ** 2 * this.blockedRatio))
      for (let i = 0; i < this.gridSize ** 2; i++) {
        let occupied = false
        let blocked = false
        let color = '#3fb547'
        if (randomOccupied.includes(i + 1)) {
          occupied = true
          color = '#949c94'
        }
        if (randomBlocked.includes(i + 1)) {
          blocked = true
          color = '#000000'
        }
        this.coordinates.push({ color, occupied, blocked })
      }
      this.tempGridSize = this.gridSize
      this.showGrid = true
    },
    onPlay () {
      console.log('not there yet .....')
    },
    hideGrid () {
      this.showGrid = false
    },
    showgrid () {
      this.showGrid = true
    },
    getRandomValues (min: number, max: number, numValues: number): number[] {
      const randomValues = [] as any

      for (let i = 0; i < numValues; i++) {
        const randomValue = Math.floor(Math.random() * (max - min + 1) + min)
        if (!randomValues.includes(randomValue)) {
          randomValues.push(randomValue)
        } else {
          i--
        }
      }
      return randomValues
    }
  },
  async mounted () {
    this.dataReady = true
  }
})
</script>
<style>
.grid {
  display: flex;
  flex-direction: column;
  align-items: center;
}

.row {
  display: flex;
}

.square {
  margin: 2px;
  color: #aa4646;
}
.background-image {
  position: absolute;
  top: 50%;
  left: 50%;
  height: 100vh;
  width: 177.77777778vh; /* 100 * 16 / 9 */
  min-width: 100%;
  min-height: 56.25vw; /* 100 * 9 / 16 */
  transform: translate(-50%, -50%);
  z-index: 0;
  pointer-events:none;
}
.generate-button {
    padding: 5px;
  font-family: agency-fb !important;
  font-size: 20px;
  width: 150px;
  height: 50px;
  border-radius: 12px;
  margin: 10px;
  margin-top: 15px;
  background-color: rgba(83, 74, 74, 0);
  border:2px solid rgb(170, 70, 70,0.3);
  pointer-events: auto;
}
.play-button {
    padding: 5px;
  font-family: agency-fb !important;
  font-size: 20px;
  width: 50px;
  height: 50px;
  border-radius: 12px;
  margin: 10px;
  margin-top: 15px;
  background-color: rgba(83, 74, 74, 0);
  border:2px solid rgb(170, 70, 70,0.3);
  pointer-events: auto;
}
.input{
      padding: 5px;
      font-family: agency-fb !important;
      font-size: 20px;
      border-radius: 12px;
      max-width: 200px;
      min-width: 100px;
      width:inherit;
      margin: 10px;
      color:aliceblue;
      background-color: rgba(83, 74, 74, 1);
      border:2px solid rgb(170, 70, 70,0.5);
      text-align: center;
      pointer-events: auto;
  }
@font-face{
  font-family: cyberFont;
  src: url("../assets/fonts/cyberspace.ttf");
}
@font-face{
    font-family: agency-fb;
    src: url("../assets/fonts/AgencyFB-Bold.ttf");
}
.main-card {
  background-color: rgba(50,50,50,0);
  color: white;
  width: fit-content;
  min-height: fit-content;
  max-height: 90vh;
  /* border:2px solid rgb(170, 70, 70,0.3); */
  margin: 0;
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  pointer-events: auto;
}
::-webkit-scrollbar {
    width: 0px;
}
.loading-leaderboard-orbit-div {
  position: absolute;
  top: 50%;
  left: 50%;
  margin: 0;
  transform: translate(-50%, -50%);
  width: 376px;
  height: 200px;
  border:2px solid rgba(170, 170, 70,0);
}
.orbit-spinner.spinner {
  margin-top: 25px;
  margin-left: 113px;
}
html, body {
  overflow: hidden !important;
  --toastify-color-light: rgba(50,50,50,0.3);
  --toastify-color-dark: rgba(50,50,50,0.3);
  --toastify-color-info: rgba(50,50,50,0.3);
  --toastify-color-success: rgba(50,50,50,0.3);
  --toastify-color-warning: rgba(50,50,50,0.3);
  --toastify-color-error: rgba(50,50,50,0.3);
  --toastify-text-color-dark: rgb(255,255,255);
  --toastify-font-family: agency-fb;
  --toastify-text-color-light: rgb(255,255,255);
  --toastify-text-color-info: #fff;
  --toastify-text-color-success: #fff;
  --toastify-text-color-warning: #fff;
  --toastify-text-color-error: #fff;
  --toastify-color-progress-info: #aa4646;
  --toastify-color-progress-success: rgb(170, 70, 70);
  --toastify-color-progress-warning:rgb(170, 70, 70);
  --toastify-color-progress-error: rgb(170, 70, 70);
  --toastify-toast-width: 500px;
  /* overflow: hidden !important; */
}
</style>
