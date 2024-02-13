<template>
  <h1>{{ header }}</h1>

  <h2 class="description">
    This collection of css and javascript animated text effects developed as Vue.js components is able to be used in different ways, some can run continuously and some are intended to wait until a triggering event like a page scroll or click. Some require Javascript, and some are CSS only.
  </h2>

  <div class="topframe">
    <div class="display"
      :style="{'font-size': fontSize + 'rem'}"  
      id="stage"
      :class="displayClass"
      v-html="inputContentStorage">
    </div>
  </div>

  <div class="topframe-spacer">
    <div class="topframe__fontsize">
      Font Size: 
      <span @click="fontSize+=.25" aria-controls="stage">Larger</span> | 
      <span @click="fontSize-=.25" aria-controls="stage">Smaller</span> | 
      <span @click="fontSize=2" aria-controls="stage">Default</span>
    </div> 
  </div>
  <textarea 
    v-model="inputContent" 
    type="input" 
    class="input">
  </textarea>

  <div class="trigger-btns">
    <component 
      v-for="(elem, i) in effects" 
      v-bind:key="i" 
      :is="effects[i].name + 'Style'" 
      :name="effects[i].name" 
      :inputContent="inputContent" 
      :displayClass="this.displayClass"
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
        v-html="effects[i].text">
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
import RansomStyle from './components/RansomStyle.vue';

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
    RansomStyle,
    LicenseText
  },
  data() { 
    return {
      effects,
      header: 'Welcome to the HyperText Party!',
      displayClass: '',
      fontSize: 2,
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
.trigger-reset {
  background-color: peachpuff;
  border:1px solid peachpuff;
  border-radius: 6px;
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
