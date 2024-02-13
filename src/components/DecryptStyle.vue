<template>
  <button 
    :class="!ready ? '' : 'trigger-ready'" 
    @click="setClass('decrypt')"
    type="button">
    {{ !ready ? name : 'Trigger Event' }}
  </button>
</template>

<script>
export default {
  name: 'DecryptStyle',
  props: ['name', 'inputContent', 'displayClass'],
  data() {
    return {
      ready: false,
      chars: "ABCDEFGHIJKLMNPQRSTUVWXYZ123456789•üπö‡#%&$*?¥∆ß?√∂Ç◊Ø£€¢!"
    }
  },
  watch: { 
    displayClass: function(resetValue) {
      if (resetValue != 'decrypt') {
        this.resetButton();
      }
    }
  },
  methods: {
    setClass: function(nuDisplayClass) {
      this.$emit('setDisplay', nuDisplayClass);
      this.decrypt(this.inputContent);
    },
    resetButton: function() {
      this.ready = false;
    },
    decrypt: function(inputContent) {
      let inputValueArray = inputContent.split('');
    // creates HTML tag helper array: [index,tag,length]
      let displayArray = [],
          helperTagArr = [],
          tmpTagArr = [],
          tagContent = '';
      for (let i=0; i <= inputValueArray.length-1; i++) {
        let tagStart = i;
        if (inputValueArray[i] == '<') {
          tmpTagArr.push(tagStart);
        }
        if (tmpTagArr.length) {
          if (inputValueArray[i] !== '>') {
            tagContent += inputValueArray[i];
          } else {
            tmpTagArr.push(tagContent + '>', tagContent.length+1);
            helperTagArr.push(tmpTagArr);
            tmpTagArr = [];
            tagContent = '';
          }
        }
        displayArray.push(this.chars[Math.floor(Math.random() * this.chars.length)]);
      }

    // uses tag array data to replace chars with tags: 
      function fixtags(displayArray) {
        let nuArray = [],
            currentTag = 0;
        for (let i=0; i < displayArray.length+1; i++) {
          if (helperTagArr.length && helperTagArr[currentTag] !== undefined) {
            if (i == helperTagArr[currentTag][0]) {
              nuArray.push(...helperTagArr[currentTag][1].split(''));
              i += helperTagArr[currentTag][2]-1;
              currentTag++;
            } else {
              nuArray.push(displayArray[i]);
            }
          } else {
            nuArray.push(displayArray[i]);
          }
        }
        return nuArray.join('');
      }
      if (this.ready == false) {
        this.$emit('setText', fixtags(displayArray));
        this.ready = true;
        return;
      }
    // replaces randoms with originals one char at a time sends to fixtags to add tags back for display at end, switches each character 2x before moving on.
      let ticker = -1,
          resultArray = [],
          ztagArr = [...helperTagArr],
      timerObj = setInterval(() => {
        ticker > displayArray.length*2 ? clearInterval(timerObj) : ticker++;
        if (ticker % 2 == 0) {
          if (ztagArr[0] !== undefined && ticker/2 == ztagArr[0][0]) {
            resultArray.push(...ztagArr[0][1].split(''));
            ticker += (ztagArr[0][2]*2)-2;
            ztagArr.shift();
          } else {
            resultArray.push(inputValueArray[ticker/2]);
          }
        }
        let tempArray = [];
        for (let i=ticker/2; i < displayArray.length-1; i++) {
          tempArray.push(this.chars[Math.floor(Math.random() * this.chars.length)]);
        }
        this.$emit('setText', fixtags([...resultArray, ...tempArray]));
      }, 35);
      this.ready = false;
    }
  }
}
</script>

<style>
.decrypt {
  text-align: left;
  font-family: 'courier';
  font-size: 1em;
}
</style>
