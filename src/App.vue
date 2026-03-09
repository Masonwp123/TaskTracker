<script setup>
import router from './router'

import Toolbar from "primevue/toolbar"
import Button from "primevue/button"
</script>

<template>
  <Toolbar class="navigation">
    <template #start>
      <div class="flex items-center gap-2">
        <Button @click="router.push('/')" label="Home"/>
        <Button @click="router.push('/about')" label="About"/>
      </div>
    </template>
  </Toolbar>
  <main>
    <RouterView/>
  </main>
</template>

<style scoped>
.navigation {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
}
.navigationtext {
  font-size: 18px;
}
</style>
