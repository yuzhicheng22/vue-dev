<script lang="ts" setup>
const props = defineProps(['title', 'eventName'])
// defineEmits([props.eventName])
const emit = defineEmits([props.eventName])
function onBtnClick() {
  emit(props.eventName)
}
</script>

<template>
  <button class="btn-common" @click="onBtnClick">
    <slot name="icon"></slot>
    <h1>{{ props.title }}</h1>
  </button>
</template>

<style scoped></style>
