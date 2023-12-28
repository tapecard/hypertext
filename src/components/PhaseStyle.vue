<template>
  <button 
    @click="setClass('phase')" 
    type="button">
    {{ name }}
  </button>
</template>

<script>
export default {
  name: 'PhaseStyle',
  props: ['name', 'inputContent'],
  methods: {
    setClass: function(nuDisplayClass) {
      this.$emit('setDisplay', nuDisplayClass);
      this.$emit('setText', this.inputContent);
    }
  }
}
</script>

<style>
.display.phase {
  text-align: center;
  width: 100%;
  font-size: 2rem;
}
.phase {
  text-align: center;
  color: rgba(0, 0, 0, 1);
  animation: phase 6s infinite;
}
@keyframes phase {
  0% {
    color: rgba(0, 0, 0, .15);
    text-shadow:
    -2px -1px 0  rgba(138, 20, 255, .25), /*purple*/
    2px 1px 0  rgba(18, 209, 8, 0.25),    /*green*/
    -1px -1px 0 rgba(9, 9, 222, 0.5),     /*blue*/ 
    -2px 2px 0  rgba(255,255,0, .25),     /*yellow*/
    0 -2px 0  rgba(227, 23, 23, 0.5);     /*red*/
    }
    33% {
      color: rgba(0, 0, 0, .15);
      text-shadow:
      2px -1px 0  rgba(255,0,0, .25),    /*red*/
      -2px 0 0  rgba(138, 20, 255, .25), /*purple*/
    1px -2px 0  rgba(8,150,19, .25),   /*green*/
    0 -1px 0  rgba(0,0,255, .25),      /*blue*/
    -1px 1px 0  rgba(255,255,0, .5);   /*yellow*/
    
    }
    66% {
      color: rgba(0, 0, 0, .15);
      text-shadow:
      1px -1px 0  rgba(255,255,0, .25),   /*yellow*/
      2px 1px 0  rgba(138, 20, 255, .25), /*purple*/
    -2px 1px 0  rgba(8,150,19, .25),    /*green*/
    2px -2px 0  rgba(0,0,255, .25),     /*blue*/
    -3px 1px 0  rgba(255,0,0, .25);     /*red*/
    }
    100% {
    color: rgba(0, 0, 0, .15);
    text-shadow: 
    -2px -1px 0  rgba(138, 20, 255, .25), /*purple*/
    2px 1px 0  rgba(18, 209, 8, 0.25),    /*green*/
    -1px -1px 0 rgba(9, 9, 222, 0.5),     /*blue*/ 
    -2px 2px 0  rgba(255,255,0, .25),     /*yellow*/
    0 -2px 0  rgba(227, 23, 23, 0.5);     /*red*/
    }
  
}
</style>