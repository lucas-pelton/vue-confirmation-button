// Made with love by Rohan Likhite

<template>
  <button
    :class="[ css, stepsComplete? 'confirmation__button--complete' : '', 1 == currentStep? 'confirmation__button--waiting' : '' ]"
    :disabled='stepsComplete'
    v-on:click='incrementStep()'>
      {{ currentMessage }}
  </button>
</template>

<script>
  export default {
    name: 'vue-confirmation-button',
    props: {
      messages: Array,
      css: {
        type: String,
        default: 'confirmation__button'
      },
    },
    data() {
      return {
        defaultSteps: [
          'Click to confirm',
          'Are you sure?',
          '✔',
        ],
        currentStep: 0,
      }
    },
    computed: {
      messageList() {
        return this.messages ? this.messages : this.defaultSteps
      },
      currentMessage() {
        return this.messageList[this.currentStep]
      },
      lastMessageIndex() {
        return this.messageList.length - 1
      },
      stepsComplete() {
        return this.currentStep === this.lastMessageIndex
      }
    },
    methods: {
      incrementStep() {
        this.currentStep++
        if (this.stepsComplete) {
          this.$emit('confirmation-success')
        }
        else {
          this.$emit('confirmation-incremented')
        }
      },
      reset() {
        this.currentStep = 0
        this.$emit('confirmation-reset')
      },
    },
  }
</script>

<style>
  .confirmation__button {
    display: block;
    background: #5B64B4;
    font-size: 0.8em;
    font-weight: 700;
    color: #ffffff;
    border-radius: 50px;
    height: 50px;
    min-width: 130px;
    padding: 0em 1em;
    outline: 0;
    cursor: pointer;
    text-transform: capitalize;
    border: 1px solid rgba(255,255,255,0.2);
    box-shadow: 0px 6px 54px rgba(71,78,152,0.5);
    -webkit-transition: background 0.3s ease-in,
                        min-width 0.1s linear,
                        box-shadow 0.2s ease-in;
    transition: background 0.3s ease-in,
                min-width 0.1s linear,
                box-shadow 0.2s ease-in;
  }
  .confirmation__button:not(.confirmation__button--complete):hover {
    box-shadow: 0px 15px 54px rgba(71,78,152,0.7);
  }
  .confirmation__button.confirmation__button--complete {
    cursor: not-allowed;
    background: #79BA7A;
    min-width: 50px;
    padding: 0em;
    font-size: 1em;
    box-shadow: 0px 6px 54px rgba(104,160,106,0.5);
    animation: icon-pop 0.3s linear 1;
  }
  @keyframes icon-pop {
      0%   {font-size: 0.1em;}
      50%  {font-size: 1.8em;}
  }
</style>
