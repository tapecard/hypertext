<template>
    <button 
      :class="!ready ? '' : 'trigger-ready'" 
      @click="setClass('decrypt')"
      type="button"
    >
      {{ ready==false ? name : 'Trigger Event' }}
    </button>
</template>

<script>
export default {
  name: 'SpeedStyle',
  props: ['name', 'inputContent', 'displayClass'],
  data() {
    return {
      ready: false,
      chars: "ABCDEFGHIJKLMNPQRSTUVWXYZ123456789•üπö‡#%&$*?¥∆ß?√∂Ç◊Ø£€¢!"
    }
  },
  watch: { 
    displayClass: function(resetValue) { // watch it
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
  // sets up the array of tags, their locations, and lengths:
      let displayArray = [];
      let arrSize = inputValueArray.length,
          tag = false,
          tagArr = [],
          tmptagArr = [],
          tagcontent = '',
          tagIndex = 0;
      for (let i=0; i <= arrSize-1; i++) {
          let tagstart = i;
          if (inputValueArray[i] == '<') {
              tag = true;
              tagIndex++;
              tmptagArr.push(tagstart);
          }
          if (tag == true) {
              if (inputValueArray[i] !== '>') {
                  tagcontent += inputValueArray[i];
              } else {
                  tmptagArr.push(tagcontent + '>', tagcontent.length+1);
                  tagArr.push(tmptagArr);
                  tag = false;
                  tmptagArr = [];
                  tagcontent = '';
              }
          }
          var xtagIndex = tagIndex;
          var xtagArr = [...tagArr];
          displayArray.push(this.chars[Math.floor(Math.random() * this.chars.length)]);
      }

  // uses tag array data to replace chars with tags: 
      function fixtags(displayArray) {
          let nuArray = [];
          for (let e=0; e < displayArray.length+1; e++) {
              if (xtagIndex !== 0) {
                  if (e == xtagArr[0][0]) {
                      nuArray.push(...xtagArr[0][1].split(''));
                      e += xtagArr[0][2]-1;
                      xtagIndex--;
                      xtagArr.shift();
                  } else {
                      nuArray.push(displayArray[e]);
                  }
              } else {
                  nuArray.push(displayArray[e]);
              }
          }
          xtagIndex = tagIndex;
          xtagArr = [...tagArr];
          return nuArray.join('');
      }
      if (this.ready == false) {
          this.$emit('setText', fixtags(displayArray));
          this.ready = true;
          return;
      }

  // replaces randoms with originals one char at a time sends to fixtags to add tags back for display at end, switched each character 2x before moving on.
      let ticker = -1,
          resultArray = [],
          ztagArr = [...tagArr];
          let timerObj = setInterval(() => {
          ticker > displayArray.length*2 ? clearInterval(timerObj) : ticker++;
          if (ticker%2 == 0) {
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
.display.decrypt {
  text-align: left;
  font-size: 2rem;
}
</style>
