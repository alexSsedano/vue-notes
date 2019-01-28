<template>
  <div id="app">
    <navB></navB>
    <router-view/>
    <foo></foo>
  </div>
  
</template>

<script>


import foo from './components/footer.vue';
import navB from './components/navBar.vue';


export default {
  components: {
   navB,
  foo,
  }
}
</script>