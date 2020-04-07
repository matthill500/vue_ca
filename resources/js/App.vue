<template>
  <div>
     <myNavbar />
    <b-container>
      <router-view />
    </b-container>
  </div>
</template>
<script>

import MyNavbar from './components/myNavbar'

export default {
  name: 'app',
  components: {
    MyNavbar
  },
  data() {
    return {

    }
  }
}
</script>
<style>
</style>
