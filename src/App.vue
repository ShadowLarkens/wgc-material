<template>
  <v-app dark>
    <v-toolbar app>
      <v-toolbar-title class="headline text-uppercase">
        <span>Weight Gain</span>
        <span class="font-weight-light">Calculator</span>
      </v-toolbar-title>
      <v-spacer></v-spacer>
    </v-toolbar>

    <v-content>
      <Calculator/>
    </v-content>
  </v-app>
</template>

<script>
import Calculator from './components/Calculator'

export default {
  name: 'App',
  components: {
    Calculator
  },
  data () {
    return {
      //
    }
  }
}
</script>
