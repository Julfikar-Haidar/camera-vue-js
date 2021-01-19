<template>
  <div id="app">
   
    <PhotoCapture
     v-model="facecapture"
     />
  </div>
</template>

<script>

import PhotoCapture from './components/PhotoCapture.vue'

export default {
  name: 'App',
  components: {
    PhotoCapture
  },
  data(){
    return {
      facecapture: null
    }
  }
}
</script>

<style>

</style>
