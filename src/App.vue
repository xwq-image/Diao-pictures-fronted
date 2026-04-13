<script setup lang="ts">
import { RouterLink, RouterView } from 'vue-router'
import BaseLayout from '@/layout/BaseLayout.vue'
import { useLoginUserStore } from '@/stores/user'
const loginUserStore = useLoginUserStore()
loginUserStore.fetchLoginUser()

</script>

<template>
  <div id="app">
    <BaseLayout> </BaseLayout>
  </div>
</template>

<style scoped></style>
