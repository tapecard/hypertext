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
      const inputValueArray = inputContent.split('');
      const displayArray = [];
      const helperTagArr = [];
      let tagStartIndex = null;
      let tagContent = '';

      for (let i=0; i <= inputValueArray.length-1; i++) {
        const char = inputValueArray[i];
        if (char === '<') {
          tagStartIndex = i;
          tagContent = '<';
        } else if (tagStartIndex !== null) {
          tagContent += char;

          if (char === '>') {
            const tagLength = tagContent.length;
            helperTagArr.push([tagStartIndex, tagContent, tagLength]);
            tagStartIndex = null;
            tagContent = '';
          }
        }
        displayArray.push(this.chars[Math.floor(Math.random() * this.chars.length)]);
      }

    // uses tag array data to replace chars with tags: 
      function fixTags(displayArray) {
        const result = [];
        let tagIndex = 0;

        for (let i = 0; i <= displayArray.length; i++) {
          const tag = helperTagArr[tagIndex];

          if (tag && i === tag[0]) {
            const [ , tagString, tagLength ] = tag;
            result.push(...tagString.split(''));
            i += tagLength - 1;
            tagIndex++;
          } else {
            result.push(displayArray[i]);
          }
        }
        return result.join('');
      }

      if (this.ready == false) {
        this.$emit('setText', fixTags(displayArray));
        this.ready = true;
        return;
      }
    // replaces randoms with originals one char at a time sends to fixtags to add tags back for display at end, switches each character 2x before moving on.
      let ticker = -1;
      let resultArray = [];
      let remainingTags = [...helperTagArr];

      const timer = setInterval(() => {
        if (ticker > displayArray.length * 2) {
          clearInterval(timer);
          return;
        }
        ticker++;
        if (ticker % 2 === 0) {
          const index = ticker / 2;

          if (remainingTags[0] && index === remainingTags[0][0]) {
            const [ , tagContent, tagLength ] = remainingTags[0];
            resultArray.push(...tagContent.split(''));
            // skip tag length (converted to ticker units)
            ticker += (tagLength * 2) - 2; 
            remainingTags.shift();
          } else {
            resultArray.push(inputValueArray[index]);
          }
        }

        const remainingRandom = Array.from({ length: displayArray.length - Math.floor(ticker / 2) - 1 }, () => {
          return this.chars[Math.floor(Math.random() * this.chars.length)];
        });

        this.$emit('setText', fixTags([...resultArray, ...remainingRandom]));
      }, 40);

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
