<template>
  <div>
    <p>{{ getName }}</p>
    <input
      :type="type"
      :name="name"
      :id="id"
      v-model="data"
      @blur="handle($event)"
    />
  </div>
</template>

<script>
export default {
  name: "Input",
  data() {
    return {
      data: "",
    };
  },
  props: {
    type: {
      type: String,
      default: "text",
    },
    name: {
      type: String,
      default: "text",
    },
    id: {
      type: String,
      default: "text",
    },
  },
  computed: {
    getName() {
      return this.name.charAt(0).toUpperCase() + this.name.slice(1);
    },
  },
  methods: {
    handle() {
      this.$emit("input-change", this.data);
    },
  },
};
</script>

<style>
input {
  border-radius: 3px;
  border: 1px solid #eee;
  width: 100%;
  height: 30px;
  margin: 0 5px 20px 0;
}
</style>
