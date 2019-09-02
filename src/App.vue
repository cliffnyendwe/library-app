<template>
  <v-app class="grey lighten-2">
    <navbar></navbar>
    <v-content class="ma-4">
      <router-view></router-view>
    </v-content>
  </v-app>
</template>

<script>
import navbar from '@/components/navbar'
export default {
  name: 'App',
  components:{
    navbar
  },
  data () {
    return {
      //
    }
  }
}
</script>
