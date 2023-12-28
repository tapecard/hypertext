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
    displayClass: function(resetValue) { // watch it
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
.display.descale {
  text-align: center;
  width: 100%;
}
.descale {
  line-height: 1;
  letter-spacing: 0;
  word-spacing: 0;
  text-align: center;
  padding-top: 30px;
  animation: scale .75s forwards;
}
@keyframes scale {
  10% {
    color: transparent;
    font-size: 620px;
    word-spacing: 0;
    line-height: 0;
    word-spacing: 0;
  }
  11% {
    color: #000;
  }
  50% {
  line-height: .3;
  }
  100% {
    font-size: 16px;
    letter-spacing: 1px;
    line-height: 1;
    word-spacing: 1px;
  }
}
</style>
