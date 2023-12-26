<template>
  <h1>{{ header }}</h1>

  <div class="descriptionspace">
    This collection of animated text-based effects can be used in different ways, some can run continuously and some are intended to sit in a waiting state until a triggering event like a page scroll or hover. Some require Javascript and some are CSS only.
  </div>

  <div class="topframe">
    <div class="display" 
        :class="displayClass"
        v-html="inputContentStorage" 
    ></div>
  </div>

  <textarea 
    v-model="inputContent" 
    type="input" 
    class="input"
  ></textarea>

  <div class="trigger-btns">
    <DecryptStyle 
      :name="effects[1].name" 
      :inputContent="inputContent" 
      @setDisplay="this.setClass($event)"
      @setText="this.setText($event)"
    />
    <PromptStyle 
      :name="effects[0].name" 
      :inputContent="inputContent" 
      @setDisplay="this.setClass($event)"
      @setText="this.setText($event)"
    />
    <SpeedStyle 
      :name="effects[4].name" 
      :inputContent="inputContent" 
      @setDisplay="this.setClass($event)" 
      @setText="this.setText($event)"
    />
    <SpectrumStyle 
      :reset="reset"
      :name="effects[3].name" 
      :inputContent="inputContent"
      @setDisplay="this.setClass($event)" 
      @setText="this.setText($event)"
    />
    <DescaleStyle 
      :reset="reset"
      :name="effects[2].name" 
      :inputContent="inputContent" 
      @setDisplay="this.setClass($event)" 
      @setText="this.setText($event)"
    />
    <PhaseStyle
      :name="effects[5].name" 
      :inputContent="inputContent" 
      @setDisplay="this.setClass($event)" 
      @setText="this.setText($event)"
    />
    <FocusStyle
      :name="effects[6].name" 
      :inputContent="inputContent" 
      @setDisplay="this.setClass($event)" 
      @setText="this.setText($event)"
    />
  </div>

  <div class="showspace">
      <div class="showspace-tile" 
          v-for="(effect, i) in effects" 
          v-bind:key="i" 
          v-html="effects[i].text"
          >
      </div>
  </div>

  <LicenseText/>
</template>

<script>
import effects from './assets/effectData.js';

import DecryptStyle from './components/DecryptStyle.vue';
import PromptStyle from './components/PromptStyle.vue';
import SpeedStyle from './components/SpeedStyle.vue';
import SpectrumStyle from './components/SpectrumStyle.vue';
import DescaleStyle from './components/DescaleStyle.vue';
import PhaseStyle from './components/PhaseStyle.vue';
import FocusStyle from './components/FocusStyle.vue';
import LicenseText from './components/LicenseText.vue';

export default {
  name: 'App',
  components: {
    DecryptStyle,
    PromptStyle,
    SpeedStyle,
    SpectrumStyle,
    DescaleStyle,
    PhaseStyle,
    FocusStyle,
    LicenseText
  },

  data() { 
    return {
      effects,
      header: 'Welcome to the HyperText Party!',
      displayClass: '',
      inputContent: 'Your Cool Text Content!',
      inputContentStorage: '',
      reset: false
    }
  },
  methods: {
    setClass: function(nuClass) {
      this.displayClass = nuClass;
    },
    setText: function(nuContent) {
      this.inputContentStorage = nuContent;
    }
  }
}


</script>

<style>
#app {
  font-family: Avenir;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}
body {
  margin: 0;
  text-align: center;
  background-color: lightsteelblue;
  font-family: arial, helvetica;
}
H1 {
  margin: 8px auto;
}
button {
  font-size: 1rem;
  margin: 5px;
  min-width: 110px;
}
textarea {
  max-width: 1440px;
  width: 87%;
  padding: 10px;
  line-height: 1;
  border-radius: 10px;
  margin-top: 10px;
  border: none;
}
.topframe {
  font-family: Helvetica, Arial, sans-serif;
  background-color: #fff;
  width: 100%;
  overflow: hidden;
  position: relative;
}
.display {
  background-color: #fefefe;
  margin: 0 auto;
  padding: 10px;
  width: 600px;
  height: 200px;
  overflow: hidden;
}
.trigger-ready {
  border-radius: 4px;
  border: 1px solid #999;
  animation: ready 5s infinite;
}
@keyframes ready {
  0% {
    background-color: #29e000;
    box-shadow: 0 0 1px #fff;
    border-color: #c1fab4;
  }
  40% {
    background-color: #c1fab4;
    box-shadow: 0 0 6px #c1fab4;
    border-color: #c1fab4;
  }
  100% {
    background-color: #29e000;
    box-shadow: 0 0 1px #fff;
    border-color: #c1fab4;
  }
}
a {
  color: #42b983;
}
.descriptionspace {
  border-top-right-radius: 12px;
  border-top-left-radius: 12px;
  padding: 10px;
  color: #000;
  text-align: left;
  display: inline-block;
  box-sizing: border-box;
  max-width: 1440px;
  margin: 0 auto;
  width: 87%;
}
.showspace {
  display: flex;
  flex-flow: row wrap;
  justify-content: center;
  width: 100%;
  max-width: 1440px;
  position: relative;
}
.showspace-tile {
  border-radius: 12px;
  padding: 10px;
  margin: 15px;
  width: 40%;
  box-sizing: border-box;
  display: inline-block;
  text-align: left;
  color: #000;
  background-color: rgba(161, 138, 184, .5);
}
</style>
