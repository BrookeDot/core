<template>
  <button @click.prevent="$emit('click')"><i class="fa fa-times"></i></button>
</template>

<script lang="ts">
import Vue from 'vue'

export default Vue.extend({})
</script>

<style lang="scss" scoped>
@import "~#/partials/_vars.scss";

button {
  display: flex;
  place-content: center;
  place-items: center;
  position: absolute;
  right: 0;
  top: 0;
  background: $colorRed;
  width: 24px;
  height: 24px;
  border-radius: 0;
  padding: 0;
  color: #fff;
  transition: opacity .3s;

  html.touch-events & {
    opacity: 1;
  }
}
</style>
