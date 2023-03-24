<template>
  <Transition name="modal">
    <div v-if="show" class="modal-mask">
      <div class="modal-wrapper">
        <div class="modal-container">
          <div class="modal-header">
            <h3>{{ title }}</h3>
          </div>

          <div class="modal-body">
              <Input name="description" type="text" id="description" @input-change="getValue" />
              <button
                class="modal-default-button"
                @click="handle"
              >Send</button>
          </div>
          <button
            class="modal-close-button"
            @click="$emit('close')"
          >X</button>
        </div>
      </div>
    </div>
  </Transition>
</template>

<script>
import Input from '../atoms/Input.vue'
export default {
    name: 'Modal',
    components: {
        Input
    },
    data() {
        return {
            inputValue: ''
        }
    },
    props: {
        title: {
            type: String,
            default: ''
        },
        show: {
            type: Boolean,
            default: false
        },
    },
    methods: {
        handle () {
            this.$emit('send', this.inputValue);
        },
        getValue(event) {
            this.inputValue = event
        }
    }
}
</script>

<style>
.modal-mask {
  position: fixed;
  z-index: 9998;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.5);
  display: table;
  transition: opacity 0.3s ease;
  text-align: left;
}

.modal-wrapper {
  display: table-cell;
  vertical-align: middle;
}

.modal-container {
  position: relative;
  width: 300px;
  margin: 0px auto;
  padding: 20px 30px;
  background-color: #fff;
  border-radius: 2px;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.33);
  transition: all 0.3s ease;
}

.modal-header h3 {
  margin-top: 0;
  color: #42b983;
}

.modal-body {
  margin: 20px 0;
}

.modal-default-button {
  float: right;
    color: #fff;
    font-size: 20px;
    background-color: #0a550a;
    padding: 2px 7px;
    border-radius: 3px;
    border: 0 solid #000;
}

.modal-close-button {
    position: absolute;
    top: -10px;
    right: -10px;
    color: #fff;
    font-size: 20px;
    background-color: #FA1313;
    padding: 2px 7px;
    border-radius: 25px;
    border: 0 solid #000;
}
.modal-enter-from {
  opacity: 0;
}

.modal-leave-to {
  opacity: 0;
}

.modal-enter-from .modal-container,
.modal-leave-to .modal-container {
  -webkit-transform: scale(1.1);
  transform: scale(1.1);
}
</style>