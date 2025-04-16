<template>
  <h1>{{ header }}</h1>

  <h2 class="description">
    This collection of css and javascript animated text effects developed as Vue.js components can be used in different ways. Some can run continuously and some are intended to wait until a triggering event like a page scroll or click. Some require Javascript, and some are CSS only.
  </h2>

  <div class="mainFrame">
    <div class="display"
      :style="{'font-size': fontSize + 'rem'}"  
      id="stage"
      :class="displayClass"
      v-html="inputContentStorage">
    </div>
  </div>

  <div class="mainFrame__content">
    <div class="mainFrame__fontsize">
      Font Size: 
      <span @click="fontSize+=.25" aria-controls="stage">Larger</span> | 
      <span @click="fontSize-=.25" aria-controls="stage">Smaller</span> | 
      <span @click="fontSize=2" aria-controls="stage">Default</span>
    </div> 

    <textarea 
      v-model="inputContent" 
      type="input" 
      class="mainFrame__input">
    </textarea>
  
    <component 
      v-for="(elem, i) in effects" 
      :key="i" 
      :is="effects[i].name + 'Style'" 
      :name="effects[i].name" 
      :inputContent="inputContent" 
      :displayClass="displayClass" 
      @setDisplay="setClass($event)" 
      @setText="setText($event)" 
    />

    <div v-if="effectDescription" v-html="effectDescription" class="effect__description"></div>
    
    <div class="mainFrame__suggestions">
      <span>
        Effects also accept inline HTML, try adding some formatting:
      </span>
      <button
      @click="inputContent='Your <b>Cool</b><br>Headline!'">Pre-formatted Text</button>
    </div>

  </div>

  <div class="showspace">
      <div 
        v-for="(effect, i) in effects" 
        class="showspace__tile" 
        :key="i" 
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
      inputContent: 'Your Cool Headline!',
      inputContentStorage: '',
      effectDescription: 'Customize the text and click the buttons above to see the effects. Buttons provide feedback on triggering and resetting each effect.'
    }
  },
  methods: {
    setClass: function(nuClass) {
      this.displayClass = nuClass;
      const iterator = effects.keys();
      for (const key of iterator) {
        if (effects[key].name.toLowerCase() == nuClass) {
          this.effectDescription = effects[key].text;
        }
      }
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
