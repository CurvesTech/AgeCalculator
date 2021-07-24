<template>
  <div>
    <AppTitle></AppTitle>
    <AppContent></AppContent>
    <AppFooter></AppFooter>
  </div>
</template>

<script>
  import AppTitle from './components/AppTitle.vue';
  import AppContent from './components/AppContent.vue';
  import AppFooter from './components/AppFooter.vue';
  export default {
    name: 'App',
    components: {
      AppTitle,
      AppContent,
      AppFooter,
    }
  }
</script>

<style>
  @import url('https://fonts.googleapis.com/css2?family=Anton&family=Lobster&family=Merriweather+Sans:wght@300&display=swap');
  * {
    padding: 0;
    margin: 0;
    font-family: 'Merriweather Sans', sans-serif;
  }
  
</style>
