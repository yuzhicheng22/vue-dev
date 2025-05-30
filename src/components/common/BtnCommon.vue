<script lang="ts" setup>
function onBtnClick() {
  console.log('click')
}
</script>

<template>
  <button class="btn-common" @click="onBtnClick">
    <slot name="icon"></slot>
    <slot name="title"></slot>
  </button>
</template>

<style scoped></style>
