<template>
  <button 
    :class="!ready ? '' : 'trigger-ready'" 
    @click="setClass('prompt-active')"
    type="button"
  >
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
      if (resetValue != 'prompt-active') {
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
        result = ' ' + '<span class="ready-prompt">_</span>';
        this.$emit('setText', result);
        this.ready = true;
        return;
      }
    // creates HTML tag helper array: [index,tag,length]
      let workText = inputContent.replace(/ /g, '_');
      function getTagArrays(workText) {
        let helperTagArr = [],
            tmpTagArr = [],
            tagContent = '';
        for (let i=0; i <= workText.length-1; i++) {
          if (workText[i] == '<') {
            tmpTagArr.push(i);
          }
          if (tmpTagArr.length > 0) {
            if (workText[i] !== '>') {
              tagContent += workText[i];
            } else {
              tmpTagArr.push(tagContent.replace(/ /g, '_') + '>', tagContent.length+1);
              helperTagArr.push(tmpTagArr);
              tmpTagArr = [];
              tagContent = '';
            }
          }
        }
        return helperTagArr;
      }

      let i = 0,
          ticker = 0,
          currentTag = 0,
          helperTagArr = getTagArrays(workText.split('')),
          txtTimer = setInterval(() => {
            ticker > inputContent.length+1 ? clearInterval(txtTimer) : ticker++;
            if (i <= workText.length+1) {
              if (helperTagArr[currentTag] != undefined && helperTagArr.length && i == helperTagArr[currentTag][0]) {
                updateStuff();
                function recursiveTagCheck() {
                  if (workText[i] === '<') {
                    result = getTagResult();
                  }
                }
                function updateStuff() {
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
                result = workText.slice(0,i-1).replace(/_/g, ' ') + '<span class="prompt">' + workText.slice(i-1,i) + '</span>';
                this.$emit('setText', result);
              }
            } else {
              result = result.slice(0,i-2) + '<span class="ready-prompt">_</span>';
              this.$emit('setText', result);
            }
          }, 70);
        this.ready = false;
    }
  }
}
</script>

<style >
.display.prompt-active {
  text-align: left;
  line-height: 1.2;
}
.prompt-active .prompt {
  color: #dafcd7;
  min-width: 14px;
  display: inline-block;
  text-align: center;
  background-color: #11a603;
  font-size: inherit;
  line-height: 1.2;
  box-shadow: 0 0 8px 5px #a5fa9d;
}
.ready-prompt {
  color: transparent;
  min-width: 14px;
  display: inline-block;
  text-align: center;
  background-color: #11a603;
  box-shadow: 0 0 8px 5px #a5fa9d;
  opacity: 1.2;
  position: absolute;
  font-size: inherit;
  line-height: 1.2;
  animation: prompt 1.3s infinite;
}
@keyframes prompt {
  0% {opacity:1;}
  50% {opacity:.2;}
  100% {opacity:1;}
}
</style>
