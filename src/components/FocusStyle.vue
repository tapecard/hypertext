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
  color: inherit;
  filter: blur(2rem);
  animation: focus 2.75s both;
}
@keyframes focus {
  0% {
    filter: blur(1.75rem);
  }
  60% {
    filter: blur(0);
  }
  80% {
    filter: blur(.15rem);
  }
  100% {
    filter: blur(0);
  }
}
</style>