<template>
  <button 
    :class="!resetMe ? '' : 'trigger-reset'"
    @click="setClass('focus')" 
    type="button">
    {{ !resetMe ? name : 'Reset Effect' }}
  </button>
</template>

<script>
export default {
  name: 'FocusStyle',
  props: ['name', 'inputContent', 'displayClass'],
  data() {
    return {
      resetMe: false
    }
  },
  watch: { 
    displayClass: function(resetValue) {
      if (resetValue != 'focus') {
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
      this.resetMe = false;
      this.$emit('setText', '');
      this.$emit('setDisplay', '');
    },
    resetButton: function() {
      this.resetMe = false;
    }
  }
}
</script>

<style>
.display.focus {
  text-align: center;
  color: rgba(0, 0, 0, 0);
  text-shadow: 0 0 30px rgba(0, 0, 0, .3);
  animation: focus 2.75s both;
}
@keyframes focus {
  0% {
    text-shadow: 0 0 40px rgba(0, 0, 0, .3);
  }
  60% {
    text-shadow: 0 0 0 rgba(0, 0, 0, 1);
  }
  80% {
    text-shadow: 0 0 5px rgba(0, 0, 0, .5);
  }
  100% {
    text-shadow: 0 0 0 rgba(0, 0, 0, 1);
  }
}
</style>