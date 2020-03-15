<template>
  <div id="app">
    <NavbarMobile v-if="windowSize()"/>
    <Navbar v-if="!windowSize()"/>
    <router-view/>
  </div>
</template>

<script>
import Navbar from '@/components/layouts/Navbar'
import NavbarMobile from '@/components/layouts/NavbarMobile'
export default {
  name: 'App',
  components: {
    Navbar,
    NavbarMobile
  },
  methods: {
    windowSize(){
      console.log(window.outerWidth)
      if(window.outerWidth < 500){
        return true
      }else{
        return false
      }
    }
  }
}
</script>

<style>
body {
  background: #ddd;
}
</style>
