<template>
  <div class="card">
    <div class="card-body">
      <div class="input-group">
        <p class="input-group fw-light">{{ description }}</p>
        <span class="input-group-text" id="basic-addon3" >{{ title }}</span>
        <input type="search" v-model="searchContact"  class="form-control">
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent } from "vue";

export default defineComponent({
  name: "FilterContact",
  props: {
    description: String,
    title: String,
  },
  emits: ["filter"],
  data() {
    return {
      searchContact:"",
    }
  },
  watch: {
    searchContact(newValue: string) {
      this.$emit("filter", newValue);
    },
  },
});
</script>
