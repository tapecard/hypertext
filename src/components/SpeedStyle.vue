<template>
  <button 
    @click="setClass('speed')" 
    type="button">
    {{ name }}
  </button>
</template>

<script>
export default {
  name: 'SpeedStyle',
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
.display.speed {
  text-align: center;
  width: 100%;
  font-size: 2rem;
}
.speed {
  font-size: 2rem;
  overflow: visible;
  text-align: center;
  transform: skewX(-22deg);
  text-shadow: 
    30px 0 0 rgba(0, 0, 0, .25), 
    -50px 0 0 rgba(0, 0, 0, .25), 
    50px 0 0 rgba(0, 0, 0, .35);
  animation: speeds 24ms infinite;
}
.speed:before {
  content: '';
  transform: skewX(-22deg);
  border-top: 2px dashed rgba(255, 255, 255, 1);
  border-bottom: 2px dashed rgba(255, 255, 255, 1);
  width: 100%;
  height: 5px;
  left: -50%;
  position: absolute;
  transform: translate(30px, 18px);
  animation: speedbefore 150ms infinite;
  z-index: 3;
}
@keyframes speeds {
  0% {
    text-shadow: 
      -2px 0 0 rgba(0, 0, 0, .3), 
      -5px 0 0 rgba(0, 0, 0, .2),
      -10px 0 0 rgba(0, 0, 0, .1),
      -18px 0 0 rgba(0, 0, 0, .075);
    }
    100% {
    text-shadow: 
      -2px 0 0 rgba(0, 0, 0, .1), 
      -5px 0 0 rgba(0, 0, 0, .2),
      -10px 0 0 rgba(0, 0, 0, .1),
      -15px 0 0 rgba(0, 0, 0, .075),

      -190px 0 0 rgba(0, 0, 0, .15),
      -50px 0 0 rgba(0, 0, 0, .15), 
      10px 0 0 rgba(0, 0, 0, .1), 
      5px 0 0 rgba(0, 0, 0, .1), 
      -100px 0 0 rgba(0, 0, 0, .25),
      -120px 0 0 rgba(0, 0, 0, .25);
    }
}
@keyframes speedbefore {
  0% {
    }
    20% {
      transform: translate(15px, 1px);
    }21% {
      transform: translate(-15px, 5px);
    }
    50% {
      transform: translate(-15px, 4px);
    }
    51% {
      transform: translate(0, -1px);
    }
    70% {
      transform: translate(-2px, 5px);
    }
    100% {
      transform: translate(-30px, 2px);
    }
}
</style>
