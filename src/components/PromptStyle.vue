<template>
  <button 
    :class="!ready ? '' : 'trigger-ready'" 
    @click="setClass('prompt')"
    type="button">
    {{ !ready ? name : 'Trigger Event' }}
  </button>
</template>

<script>
export default {
  name: 'PromptStyle',
  props: ['name', 'inputContent', 'displayClass'],
  data() {
    return {
      ready: false
    }
  },
  watch: { 
    displayClass: function(resetValue) {
      if (resetValue != 'prompt') {
        this.resetButton();
      }
    }
  },
  methods: {
    setClass: function(nuDisplayClass) {
      this.$emit('setDisplay', nuDisplayClass);
      this.prompt(this.inputContent);
    },
    resetButton: function() {
      this.ready = false;
    },
    prompt: function(inputContent) {
      let result = '';
      if (this.ready == false) {
        result = ' ' + '<span class="prompt__empty">_</span>';
        this.$emit('setText', result);
        this.ready = true;
        return;
      }
    // creates HTML tag helper array: [index,tag,length]
      let workText = inputContent.replace(/ /g, '_');
      function getTagArrays(workText) {
        const tagData = [];
        let tagStart = null;
        let tagContent = '';

        for (let i = 0; i < workText.length; i++) {
          const char = workText[i];

          if (char === '<') {
            tagStart = i;
            tagContent = '<';
          } else if (tagStart !== null) {
            tagContent += char;

            if (char === '>') {
              const cleanedTag = tagContent.replace(/ /g, '_');
              tagData.push([tagStart, cleanedTag, tagContent.length]);
              tagStart = null;
              tagContent = '';
            }
          }
        }
        return tagData;
      }

      let i = 0,
          ticker = 0,
          currentTag = 0;
      const helperTagArr = getTagArrays(workText.split(''));
      const txtTimer = setInterval(() => {
        ticker > inputContent.length+1 ? clearInterval(txtTimer) : ticker++;
        if (i <= workText.length+1) {
          if (helperTagArr[currentTag] != undefined && helperTagArr.length && i == helperTagArr[currentTag][0]) {
            updateStuff();
            function recursiveTagCheck() {
              if (workText[i] === '<') {
                result = getTagResult();
              }
            }
            function updateStuff () {
              result = getTagResult();
              i += helperTagArr[currentTag][2];
              currentTag++;
              result = getTagResult();
              recursiveTagCheck();
            }
            function getTagResult() {
              return workText.slice(0,i).replace(/_/g, ' ');
            }
          } else {
            i++;
            result = workText.slice(0,i-1).replace(/_/g, ' ') + '<span class="prompt__insert">' + workText.slice(i-1,i) + '</span>';
            this.$emit('setText', result);
          }
        } else {
          result = result.slice(0,i-2) + '<span class="prompt__empty">_</span>';
          this.$emit('setText', result);
        }
      }, 70);
    this.ready = false;
    }
  }
}
</script>

<style >
.prompt {
  text-align: left;
  line-height: 1.1;
}
.prompt .prompt__insert {
  color: #dafcd7;
  min-width: .5em;
  display: inline-block;
  text-align: center;
  background-color: #11a603;
  font-size: inherit;
  line-height: 1.1;
  box-shadow: 0 0 5px 3px #a5fa9d;
}
.prompt__empty {
  color: transparent;
  min-width: .5em;
  display: inline-block;
  text-align: center;
  background-color: #11a603;
  box-shadow: 0 0 5px 3px #a5fa9d;
  opacity: 1;
  position: absolute;
  font-size: inherit;
  line-height: 1.1;
  animation: prompt 1.3s infinite;
}
@keyframes prompt {
  0% {opacity:1;}
  50% {opacity:.2;}
  100% {opacity:1;}
}
</style>
