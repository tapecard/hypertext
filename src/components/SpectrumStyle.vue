<template>
  <button 
    :class="!resetMe ? '' : 'trigger-reset'"
    @click="setClass('spectrum')" 
    type="button">
    {{ !resetMe ? name : 'Reset Effect' }}
  </button>
</template>

<script>
export default {
  name: 'SpectrumStyle',
  props: ['name', 'inputContent', 'displayClass'],
  data() {
    return {
      resetMe: false
    }
  },
  watch: { 
    displayClass: function(resetValue) {
      if (resetValue != 'spectrum') {
        this.resetButton();
      }
    }
  },
  methods: {
    setClass: function(nuDisplayClass) {
      if (this.resetMe == true) {
        this.resetEffect();
      } else {
        this.$emit('setDisplay', nuDisplayClass);
        this.$emit('setText', this.inputContent);
        this.resetMe = true;
      }
    },
    resetEffect: function() {
      this.$emit('setText', '');
      this.$emit('setDisplay', '');
      this.resetMe = false;
    },
    resetButton: function() {
      this.resetMe = false;
    }
  }
}
</script>

<style>
.display.spectrum {
  text-align: center;
  color: currentColor;
  animation: spectrum 1.5s normal;
}
@keyframes spectrum {
  0% {
    color: rgba(0, 0, 0, 0);
    text-shadow: 
    -20px 15px 0  rgba(138, 20, 255, 0), /*purple*/
    30px 0 0  rgba(18, 209, 8, 0),       /*green*/
    15px -20px 0 rgba(227, 142, 5, .1),  /*orange*/
    0 -20px 0 rgba(9, 9, 222, 0),        /*blue*/
    0 20px 0  rgba(255,255,0, .2),       /*yellow*/
    -30px 0 0  rgba(227, 23, 23, 0);     /*red*/
  }
  80% {
    color: rgba(0, 0, 0, 0);
  }
  90% {
    color: rgba(0, 0, 0, 0.5);
    text-shadow:
    0 0 0  rgba(138, 20, 255, .5), /*purple*/
    0 0 0  rgba(8,150,19, .5),     /*green*/
    0 0 0 rgba(227, 142, 5, .5),   /*orange*/
    0 0 0  rgba(0,0,255, .5),      /*blue*/
    0 0 0  rgba(255,255,0, .75),   /*yellow*/
    0 0 0  rgba(255,0,0, .75);     /*red*/
  }
  100% {
    color: currentColor;
  }
}
</style>
