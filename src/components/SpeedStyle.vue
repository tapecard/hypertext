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
.speed {
  text-align: center;
  overflow: visible;
  animation: speeds 24ms infinite;
}
@keyframes speeds {
  0% {
    transform: skewX(-22deg);
    text-shadow: 
      -.38em 0 0 rgba(0, 0, 0, .3), 
      -.16em 0 0 rgba(0, 0, 0, .2),
      -.35em 0 0 rgba(0, 0, 0, .1),
      -.75em 0 0 rgba(0, 0, 0, .075);
    }
    100% {
      transform: skewX(-24deg);
    text-shadow: 
      -.41em 0 0 rgba(0, 0, 0, .01), 
      -.21em 0 0 rgba(0, 0, 0, .02),
      -.06em 0 0 rgba(0, 0, 0, .4),
      -0.8em 0 0 rgba(0, 0, 0, .075),

      -1.6em 0 0 rgba(0, 0, 0, .15),
      -2em 0 0 rgba(0, 0, 0, .05), 
      .43em 0 0 rgba(0, 0, 0, .1), 
      .11em 0 0 rgba(0, 0, 0, .15), 
      -4em 0 0 rgba(0, 0, 0, .15),
      -5em 0 0 rgba(0, 0, 0, .15);
    }
}
</style>
