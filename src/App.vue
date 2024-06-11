<template>
  <div id="app">
    <div :class="['bag', { 'bag-burst': ended }]"></div>
    <div id="bag-health">
      <div :class="['bag-health-bar', healthColor]" :style="{ width: bagHealthPercentage }"></div>
    </div>
    <div id="power-meter">
      <input type="range" v-model="punchPower" min="1" max="20" />
      <span>Punch Power: {{ punchPower }}</span>
    </div>
    <div>
      <button @click="punch" v-show="!ended" aria-label="Punch the bag">Punch</button>
      <button @click="reset" aria-label="Reset the game">Restart</button>
    </div>
  </div>
</template>

<script>
export default {
  name: 'App',
  data() {
    return {
      bagHealth: 100,
      ended: false,
      punchPower: 10,
    };
  },
  computed: {
    bagHealthPercentage() {
      return this.bagHealth + '%';
    },
    healthColor() {
      if (this.bagHealth > 50) {
        return 'healthy';
      } else if (this.bagHealth > 20) {
        return 'medium';
      } else {
        return 'critical';
      }
    },
  },
  methods: {
    punch() {
      if (this.bagHealth > 0) {
        this.bagHealth = Math.max(this.bagHealth - this.punchPower, 0);
        if (this.bagHealth === 0) {
          this.ended = true;
        }
      }
    },
    reset() {
      this.bagHealth = 100;
      this.ended = false;
      this.punchPower = 10;
    },
  },
};
</script>

<style>
* {
  margin: 0;
  padding: 0;
}
#app {
  text-align: center;
  font-family: Arial, sans-serif;
  background-color: #f0f0f0;
  padding: 20px;
}
.bag {
  width: 200px;
  height: 400px;
  background: url('./assets/bag.png') center no-repeat;
  background-size: 80%;
  margin: 0 auto;
  transition: background-image 0.3s ease-in-out;
}
.bag-burst {
  background-image: url('./assets/bag-burst.png');
}
#bag-health {
  width: 200px;
  border: 2px solid #eeff00e7; /* Changed border color */
  margin: 20px auto;
  height: 20px;
  background-color: white;
  border-radius: 15px; /* Added border radius */
}
.bag-health-bar {
  height: 100%;
  transition: width 0.3s ease-in-out;
  border-radius: 10px;
}
.healthy {
  background-color: green;
}
.medium {
  background-color: orange;
}
.critical {
  background-color: red;
}
#power-meter {
  margin: 20px auto;
  width: 200px;
}
input[type='range'] {
  width: 100%;
}
button {
  padding: 10px 20px;
  margin: 10px;
  font-size: 16px;
  cursor: pointer;
  border: none;
  background-color: #007bff;
  color: white;
  border-radius: 5px;
  transition: background-color 0.3s;
}
button:hover {
  background-color: #0056b3;
}
button:focus {
  outline: none;
  box-shadow: 0 0 0 3px rgba(0, 123, 255, 0.5);
}
</style>
