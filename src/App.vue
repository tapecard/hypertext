<template>
  <h1>{{ header }}</h1>

  <h2 class="description">
    This collection of css and javascript animated text effects developed as Vue.js components and formatted as a playable demonstration site. Able to be used in different ways, some can run continuously and some are intended to wait until a triggering event like a page scroll or click. Some require Javascript, and some are CSS only.
  </h2>

  <div class="topframe">
    <div class="display" 
        :class="displayClass"
        v-html="inputContentStorage" 
    ></div>
  </div>
  <div class="topframe-spacer"></div>
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
      :name="effects[3].name" 
      :inputContent="inputContent"
      @setDisplay="this.setClass($event)" 
      @setText="this.setText($event)"
    />
    <DescaleStyle 
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
  
  <div class="suggestions">
    Effects also accept inline HTML, try adding some formatting:
    <button
    @click="this.inputContent='Your <b>Cool</b><br>Text Content!'">Pre-formatted Text</button>
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
      inputContentStorage: ''
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
H1 {
  margin: 8px auto;
  font-size: 2rem;
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
  margin-top: 0;
  border: none;
}
.description {
  border-top-right-radius: 12px;
  border-top-left-radius: 12px;
  padding: 16px;
  color: #000;
  text-align: left;
  display: inline-block;
  box-sizing: border-box;
  width: 87%;
  font-weight: normal;
  font-size: 1rem;
  background-color: #d0e0f5;
  width: 84%;
}
.topframe-spacer {
  background-color: #d0e0f5;
  width: 84%;
  height: 10px;
  position: relative;
  display: block;
  margin: 0 auto;
}
.topframe {
  font-family: Helvetica, Arial, sans-serif;
  background-color: #fff;
  width: 90%;
  overflow: hidden;
  position: relative;
  border-radius: 12px;
  margin: 0 auto;
}
.display {
  background-color: #fefefe;
  margin: 0 auto;
  padding: 10px;
  width: 600px;
  height: 200px;
  overflow: hidden;
}
.trigger-btns {
  background-color: #d0e0f5;
  width: 84%;
  text-align: center;
  margin: 0 auto;
  padding-top:5px;
}
.suggestions {
  margin: 0 auto;
  padding: 5px;
  width: 84%;
  background-color: #d0e0f5;
  border-bottom-left-radius: 16px;
  border-bottom-right-radius: 16px;
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
</style>
