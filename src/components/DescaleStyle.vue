<template>
  <button 
    @click="setClass('descale')" 
    type="button">
    {{name}}
  </button>
</template>

<script>
export default {
  name: 'DescaleStyle',
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
