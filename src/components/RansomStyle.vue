<template>
  <button 
    :class="!resetMe ? '' : 'trigger-reset'"
    @click="setClass('ransom')" 
    type="button">
    {{ !resetMe ? name : 'Reset Effect' }}
  </button>
</template>

<script>
export default {
  name: 'ransomStyle',
  props: ['name', 'inputContent', 'displayClass'],
  data() {
    return {
      resetMe: false,
      styles:'abcdefghijklmnopqrstuvwxyz'
    }
  },
  watch: { 
    displayClass: function(resetValue) {
      if (resetValue != 'ransom') {
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
        this.ransom(this.inputContent, this.styles);
        this.resetMe = true;
      }
    },
    ransom: function(inputContent, styles) {
      let inputValueArray = inputContent.split('');
      inputValueArray.forEach(function(item, index, inputValueArray) {
        let styleClass = styles[Math.floor(Math.random() * styles.length)];
        if (item === ' ') {
          inputValueArray[index] = '<span class="xx">' + item + '</span>';
        } else {
          inputValueArray[index] = '<span class="' + styleClass + '">' + item + '</span>';
        }
        
      });
      console.log(inputValueArray);
      this.$emit('setText', inputValueArray.toString().replace(/,/g, ''));
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
.display.ransom {
  text-align: center;
  width: 100%;
  font-size: 32px;
}
.ransom {
  line-height: 1;
  letter-spacing: 0;
  word-spacing: 0;
  text-align: center;
  padding-top: 30px;
  animation: ransom .75s forwards;
}
.ransom span {
  display: inline-block;
  letter-spacing: -2px;
  border: 1px solid #eee;
}
.ransom .xx {
  display: inline-block;
  padding: 0 .25em;
  letter-spacing: normal;
  border: none;
}
.ransom .a {
  font-family: times, times new roman, serif;
  background-color: #d1d1d1;
  color: #00f;
  font-size: 1.3em;
  text-transform: uppercase;
}
.ransom .b {
  font-family: Arial, helvetica, sans-serif;
  font-weight: bold;
  color: #888;
  border: 1px solid #d1d1d1;
  background-color: #fefefe;
  line-height: 1;
  font-size: 1em;
  transform: skew(5deg);
}
.ransom .c {
  font-family: times, times new roman, serif;
  font-style: italic;
  font-weight: normal;
  color: #f00;
  border: 1px solid #d1d1d1;
  background-color: #fefefe;
  font-size: 1.2em;
  padding: 2px .01em;
  line-height: .8;
  vertical-align: middle;
  transform: rotate(-4deg);
}
.ransom .d {
  font-family: Courier;
  font-style: normal;
  font-weight: lighter;
  color: #000;
  background-color: #f00;
  font-size: 1.1em;
  text-transform: uppercase;
  padding: 0 .15em 0 0 !important;
  transform: rotate(4deg);
}
.ransom .e {
  font-family: Arial, helvetica, sans-serif;
  background: linear-gradient(0deg, rgba(131,58,180,.75) 0%, rgba(253,29,29,.75) 50%, rgba(252,176,69,.75) 100%);
  color: #fff;
  font-weight: bold;
  font-size: 1.1em;
  padding: .01em .02em 0 0;
  line-height: 1;
  margin: 0 -0.01em;
  transform: rotate(2deg);
  text-transform: uppercase;
}
.ransom .f {
  font-family: Verdana, sans-serif;
  font-weight: bold;
  color: #000;
  border: none;
  box-shadow: -1px 1px 0 rgba(0,0,249,.25);
  background: linear-gradient(0deg, rgba(255,255,255,1) 0%, rgba(216,234,249,1) 100%);
  line-height: 1;
  transform: rotate(1deg);
  padding: 0 3px .03em;
  font-size: 1.4rem;
}
.ransom .g {
  font-family: times, times new roman, serif;
  font-weight: bold;
  color: #ffbaba;
  background: linear-gradient(0deg, rgba(9,69,13,1) 0%, rgba(98,204,108,1) 100%);;
  font-size: 1.2em;
  text-shadow: 1px 1px 3px rgba(0, 0, 0, .70) ;
  padding: .06em;
  transform: rotate(1deg);
  line-height: 1.05;
  vertical-align: middle;
}
.ransom .h {
  font-family: Courier;
  font-style: normal;
  font-weight: lighter;
  color: #fff;
  background-color: #000;
  font-size: 1.6em;
  text-transform: lowercase;
  transform: rotate(-3deg);
}
.ransom .h {
  font-family: Courier;
  font-style: normal;
  font-weight: lighter;
  color: #000;
  font-style: italic;
  background-color: #fff;
  box-shadow: 1px 1px 0 rgba(0, 0, 0, .30) ;
  font-size: 1.4em;
  text-transform: lowercase;
  transform: rotate(3deg);
  padding: 0 .01em 0 0;
  letter-spacing: .01em;
}
.ransom .i {
  font-family: Garamond, serif;
  font-style: normal;
  font-size: 1.1em;
  transform: rotate(1deg);
}
.ransom .j {
  font-family: Verdana, sans-serif;
  font-style: normal;
  font-size: .9em;
}
.ransom .k {
  font-family: Trebuchet, sans-serif;
  font-weight: bold;
  font-size: 1.2em;
  text-transform: lowercase;
  color: transparent;
  background-clip: text;
  background-image: linear-gradient(180deg, rgba(197,116,252,1) 32%, rgba(25,0,0,1) 74%);
}
.ransom .l {
  font-family: Garamond, serif;
  font-style: italic;
  font-size: 1.4em;
  background-color: #555;
  color: #afc9fa;
  padding: .03em;
  line-height: .75;
  letter-spacing: 0.02em;
}
.ransom .m {
  font-family: Verdana, sans-serif;
  font-style: oblique;
  font-size: 1em;
  font-weight: bold;
  background-color: #c7ffc4;
  padding: 0 0 .3em 0;
  line-height: .7;
  color: #11ad09;
}
.ransom .n {
  font-family: Trebuchet, sans-serif;
  font-weight: lighter;
  background-color: #525705;
  color:#fff;
  text-transform: lowercase;
  font-size: 1.1em;
  padding: 0 .08em 0 .03em;
  transform: rotate(-8deg) skew(-13deg);
  letter-spacing: 0.01em;
}
.ransom .o {
  font-family: Arial, sans-serif;
  font-style: italic;
  font-size: .8em;
  font-weight: bold;
  background-color: #631212;
  padding: 0 .2em 0 0;
  line-height: .9;
  color: #d4c5c5;
}
.ransom .p {
  font-family: Courier;
  font-style: normal;
  font-weight: bold;
  color: #000;
  background-color: #efefef;
  font-size: 1.2em;
  margin-right: -.01em;
  text-transform: uppercase;
  padding: 0 .05em 0 0 !important;
  transform: rotate(-4deg);
}
.ransom .q {
  font-family: Courier;
  font-style: normal;
  font-weight: normal;
  color: #efefef;
  text-shadow: .02em .02em 0 #f00, -.02em .02em 0 #f00, .02em -.02em 0 #f00, -.02em -.02em 0 #f00;
  background-color: #fefefe;
  font-size: 1.2em;
  margin-right: -.01em;
  text-transform: uppercase;
  padding: 0 .05em 0 0 !important;
  transform: rotate(-4deg);
}
.ransom .r {
  font-family: Verdana, sans-serif;
  font-weight: normal;
  color: #fff;
  border: none;
  box-shadow: -.01em .02em 0 rgba(0,0,249,.25);
  text-shadow: .05em .05em 0 #000, 0 0 0 #000;
  background-color: #dedede;
  line-height: 1;
  transform: rotate(1deg);
  padding: 0 .16em .04em 0;
  font-size: 1em;
}
/*.ransom span:nth-child(1) {
  text-transform: uppercase;
  transform: rotate(2deg);
  font-size: 1.4em;
}*/
/*.ransom span:nth-child(3) {
  font-size: 1.2em;
  font-style: italic;
  padding: 0 .12em 0 0;
  transform: rotate(1deg);
}*/

/*@keyframes ransom {
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
}*/
</style>
