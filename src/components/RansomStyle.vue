<template>
  <button 
    :class="!ready ? '' : 'trigger-ready'" 
    @click="setClass('ransom')" 
    type="button">
    {{ !ready ? name + ' Note' : 'Trigger Event' }}
  </button>
</template>

<script>
export default {
  name: 'ransomStyle',
  props: ['name', 'inputContent', 'displayClass'],
  data() {
    return {
      ready: false,
      styles:'abcdefghijklmnopqrstuvwxyz',
      helperTagArr:[],
      ransomRepeater: false
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
      this.$emit('setDisplay', nuDisplayClass);
      this.ransomSetup(this.inputContent, this.styles);
    },
    resetButton: function() {
      this.ready = false;
      clearInterval(this.ransomRepeater);
      this.ransomRepeater = false;
    },
  // creates HTML tag helper array: [index,tag,length]
    ransomSetup: function(inputContent, styles) {
      let tmpTagArr = [],
          tagContent = '';
      for (let i=0; i <= inputContent.length-1; i++) {
        if (inputContent[i] === '<') {
          tmpTagArr.push(i);
        }
        if (tmpTagArr.length) {
          if (inputContent[i] !== '>') {
            tagContent += inputContent[i];
          } else {
            tmpTagArr.push(tagContent + '>', tagContent.length+1);
            this.helperTagArr.push(tmpTagArr);
            tmpTagArr = [];
            tagContent = '';
          }
        }
      }
      this.ransomRun();
    },
  // recombines tags for output:
    ransom: function(inputContent, styles) {
      let inputValueArray = inputContent.split(''),
          outputArray = [],
          currentTag = 0;
      for (let i=0; i <= inputContent.length; i++) {
        if (this.helperTagArr.length && this.helperTagArr[currentTag] != undefined &&  i === this.helperTagArr[currentTag][0]) {
          updateStuff(this.helperTagArr);
          recursiveTagCheck(this.helperTagArr);
          function recursiveTagCheck(helperTagArr) {
            if (inputContent[i] === '<') {
              updateStuff(helperTagArr);
              recursiveTagCheck(helperTagArr);
            }
          }
          function updateStuff(helperTagArr) {
            outputArray.push(helperTagArr[currentTag][1]);
            i += helperTagArr[currentTag][2];
            currentTag++;
          }
        }
        if (inputValueArray[i] === ' ') {
          outputArray.push('<span class="xx">' + inputValueArray[i] + '</span>');
        } else if (inputValueArray[i] != undefined) {
          let styleClass = styles[Math.floor(Math.random() * styles.length)];
          outputArray.push('<span class="' + styleClass + '">' + inputValueArray[i] + '</span>');
        }
      }
      this.$emit('setText', outputArray.toString().replace(/,/g, ''));
    },
    ransomRun: function() {
      if (!this.ready && !this.ransomRepeater) {
        this.ready = true;
        this.ransom(this.inputContent, this.styles);
      } else {
        if (!this.ransomRepeater) {
          this.ready = false;
          this.ransomRepeater = setInterval(() => {
            this.ransom(this.inputContent, this.styles);
          },250);
        } else {
          this.resetButton();
        }
      }
    }
  }
}
</script>

<style>
.ransom {
  text-align: center;
  width: 100%;
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
  background-color: #ededed;
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
  padding: 1px .01em;
  line-height: .8;
  vertical-align: middle;
  transform: rotate(-4deg);
}
.ransom .d {
  font-family: Courier;
  font-style: normal;
  font-weight: bold;
  color: #000;
  background-color: #f00;
  font-size: 1.1em;
  text-transform: uppercase;
  padding: 0 .05em 0 0 !important;
  transform: rotate(4deg);
}
.ransom .e {
  font-family: Arial, helvetica, sans-serif;
  background: linear-gradient(0deg, rgba(29, 30, 110,.75) 0%, rgba(253,29,29,.75) 60%, rgba(252,176,69,.75) 100%);
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
  font-size: 1.4em;
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
  background-image: linear-gradient(180deg, rgba(197,116,252,1) 0%, rgba(25,0,0,1) 100%);
}
.ransom .l {
  font-family: Garamond, serif;
  font-style: italic;
  font-size: 1.4em;
  background-color: #555;
  color: #afc9fa;
  padding: .03em;
  line-height: .85;
  letter-spacing: 0.02em;
}
.ransom .m {
  font-family: Verdana, sans-serif;
  font-style: oblique;
  font-size: 1em;
  background-color: #f0faf0;
  padding: 0 0 .3em 0;
  line-height: .7;
  color: #11ad09;
}
.ransom .n {
  font-family: Trebuchet, sans-serif;
  font-weight: lighter;
  background-color: #aaa;
  color:#fff;
  text-transform: lowercase;
  font-size: 1.1em;
  padding: 0 .08em 0 .03em;
  transform: rotate(-8deg) skew(-10deg);
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
  font-weight: lighter;
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
.ransom .s {
  font-family: Courier;
  font-style: normal;
  font-weight: normal;
  color: #efefef;
  text-shadow: .02em .02em 0 #f00, -.02em .02em 0 #f00, .02em -.02em 0 #f00, -.02em -.02em 0 #f00;
  background-color: #fefefe;
  font-size: 1.2em;
  margin-right: -.01em;
  text-transform:lowercase;
  padding: 0 .05em 0 0 !important;
  transform: rotate(-4deg);
}
.ransom .t {
  font-family: Verdana, sans-serif;
  font-style: italic;
  font-size: 1.2em;
  color: #029fb0;
  vertical-align: middle;
  background-color: #ededed;
  padding: 0 .01em .04em 0;
}
.ransom .u {
  font-family: Trebuchet, sans-serif;
  font-weight: normal;
  background-color: #000;
  color:#ebf55f;
  font-size: 0.9em;
  padding: 0 .08em 0 .03em;
  transform: rotate(2deg);
  letter-spacing: 0.01em;
}
.ransom .v {
  font-family: Helvetica, Arial, sans-serif;
  font-weight: normal;
  color: #888;
  border: 1px solid #d1d1d1;
  background-color: #fefefe;
  line-height: 1;
  font-size: 1.1em;
  transform: skew(-2deg);
}
.ransom .w {
  font-family: times new roman, times, serif;
  font-style: normal;
  color: #000;
  background-color: #fcffd9;
  font-size: .9em;
  padding: .2em .01em;
  margin: 0 .02em;
  line-height: .8;
  vertical-align: middle;
  transform: rotate(2deg);
}
.ransom .x {
  font-family: Trebuchet, sans-serif;
  font-style: normal;
  font-weight: normal;
  color: #000;
  background-color: #fcffd9;
  font-size: .9em;
  padding: .2em .01em;
  margin: 0 .02em;
  line-height: .9;
  vertical-align: middle;
  transform: rotate(-1deg);
}
.ransom .y {
  font-family: Garamond, serif;
  font-style: normal;
  font-weight: normal;
  color: #000;
  font-size: .9em;
  padding: .2em .01em;
  margin: 0 .02em;
  line-height: .9;
  vertical-align: middle;
  transform: rotate(2deg);
}
.ransom .z {
  font-family: Verdana, sans-serif;
  font-style: normal;
  color: #7d0c1a;
  font-size: 1em;
  background-color: #f2edee;
}
</style>
