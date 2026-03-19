<script setup lang="ts">
import AppHeader from './components/AppHeader.vue';
import MainNews from './sections/MainNews.vue';
</script>

<template>
  <div class="app-container">
    <AppHeader />

    <main class="main-content">
      <MainNews />
    </main>
  </div>
</template>

<style lang="scss">
.app-container {
  min-height: 100vh;
  display: flex;
  flex-direction: column;
}

.main-content {
  max-width: 1208px;
  margin: 0 auto;
  flex: 1;
  margin-bottom: 40px;
}
</style>