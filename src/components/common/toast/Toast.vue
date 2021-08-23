<template>
  <div class="toast" v-show="isShow">
    <div>{{message}}</div>
  </div>
</template>

<script>
  export default {
    name: 'Toast',
    // props: {
    //   message: {
    //     type: String,
    //     default: ''
    //   },
    //   show: {
    //     type: Boolean,
    //     default: false
    //   }
    // },
    data() {
      return {
        message: '',
        isShow: false
      }
    },
    methods: {
      show(message, duration) {
        this.isShow = true;
        this.message = message;

        setTimeout(() => {
          this.isShow = false;
          this.message = ''
        }, duration)
      }
    },
  }
</script>

<style>
  .toast {
    position: fixed;
    top: 50%;
    left: 50%;
    color: #fff;
    font-size: 12px;
    border-radius: 10px;
    transform: translate(-50%, -50%);
    padding: 7px 20px;
    background-color: rgba(0, 0, 0, 0.6);
    z-index: 10;
  }
</style>