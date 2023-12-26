<template>
  <button @click="setClass('focus')" type="button">
    {{name}}
  </button>
</template>

<script>
export default {
  name: 'FocusStyle',
  props: ['name', 'inputContent'],
  methods: {
    setClass: function(displayClass) {
      this.$emit('setDisplay', displayClass);
      this.$emit('setText', this.inputContent);
    }
  }
}
</script>

<style>
.display.focus {
  text-align: center;
  width: 100%;
  font-size: 2rem;
}
.focus {
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