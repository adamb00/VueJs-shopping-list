<script setup>
   import HeaderComponent from './components/HeaderComponent.vue';
   import InputComponent from './components/InputComponent.vue';
</script>

<template>
   <header>
      <div class="header">
         <HeaderComponent />
      </div>
   </header>
   <main class="container">
      <InputComponent />
   </main>
</template>
<style>
   .header {
      display: flex;
      justify-content: center;
      padding: 1rem;
      font-size: 2.2rem;
   }
   .container {
      display: flex;
      justify-content: space-around;
   }
</style>
