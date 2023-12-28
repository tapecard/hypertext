<template>
  <button 
    :class="!ready ? '' : 'trigger-ready'" 
    @click="setClass('prompt-active')"
    type="button"
  >
    {{ready==false ? name : 'Trigger Event'}}
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
    displayClass: function(resetValue) { // watch it
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

      let workText = inputContent.replace(/ /g, '_');
      function getTagArrays(workText) {
      let arrSize = workText.length,
        tag = false,
        tagArr = [],
        tmptagArr = [],
        tagcontent = '';
        for (let i=0; i <= arrSize-1; i++) {
          if (workText[i] == '<') {
            tag = true;
            tmptagArr.push(i);
          }
          if (tag == true) {
              if (workText[i] !== '>') {
                tagcontent += workText[i];
              } else {
                tmptagArr.push(tagcontent.replace(/ /g, '_') + '>', tagcontent.length+1);
                tagArr.push(tmptagArr);
                tag = false;
                tmptagArr = [];
                tagcontent = '';
              }
           }
        }
        return tagArr;
      }

      let txNum = 0,
          ticker = 0,
          xtagArr = getTagArrays(workText.split('')),
          txtTimer = setInterval(() => {
            ticker > this.inputContent.length+1 ? clearInterval(txtTimer) : ticker++;
            if (txNum <= workText.length+1) {
                if (xtagArr.length && txNum == xtagArr[0][0]) {
                    var currentTag = xtagArr[0][1];
                    txNum += xtagArr[0][2];
                    xtagArr.shift();
                    result = workText.slice(0,txNum).replace(/_/g, ' ') + currentTag 
                    this.$emit('setText', result);
                } else {
                    txNum++;
                    result = workText.slice(0,txNum-1).replace(/_/g, ' ') + '<span class="prompt">' + workText.slice(txNum-1,txNum) + '</span>';
                    this.$emit('setText', result);
                }
            } else {
                result = result.slice(0,txNum-2) + '<span class="ready-prompt">_</span>';
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
  font-size: 2rem;
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
