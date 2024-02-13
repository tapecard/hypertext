<template>
  <button 
    :class="!resetMe ? '' : 'trigger-reset'"
    @click="setClass('descale')" 
    type="button">
    {{ !resetMe ? name : 'Reset Effect' }}
  </button>
</template>

<script>
export default {
  name: 'DescaleStyle',
  props: ['name', 'inputContent', 'displayClass'],
  data() {
    return {
      resetMe: false
    }
  },
  watch: { 
    displayClass: function(resetValue) {
      if (resetValue != 'descale') {
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
        this.$emit('setText', '<span>'+this.inputContent+'</span>');
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
.descale span {
  text-align: center;
  animation: scale .75s forwards;
  color: currentColor;
}
@keyframes scale {
  10% {
    font-size: 30em;
    line-height: 0;
    word-spacing: 0;
  }
  50% {
  line-height: .3;
  }
  100% {
    font-size: .875em;
    letter-spacing: .01em;
    line-height: 1;
    word-spacing: .01em;
  }
}
</style>
