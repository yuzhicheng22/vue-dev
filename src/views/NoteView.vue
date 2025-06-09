<script setup lang="ts">
import BtnCommon from '@/components/common/BtnCommon.vue'
import DocumentationIcon from '@/components/icons/IconDocumentation.vue'
import { ref } from 'vue'

const fontSize = ref(1)
</script>

<template>
  <h1>Note</h1>
  <div :style="{ fontSize: fontSize + 'rem' }">Content</div>
  <BtnCommon title="AddFontSize" eventName="AddFontSize" @AddFontSize="fontSize += 0.2">
    <template #icon>
      <DocumentationIcon />
    </template>
    <!-- <template #title> Select </template> -->
  </BtnCommon>
  <BtnCommon
    title="IncreaseFontSize"
    eventName="IncreaseFontSize"
    @IncreaseFontSize="fontSize -= 0.2"
  >
    <template #icon>
      <DocumentationIcon />
    </template>
    <!-- <template #title> Select </template> -->
  </BtnCommon>
</template>
<style></style>
