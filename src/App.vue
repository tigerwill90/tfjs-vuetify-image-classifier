<template>
  <v-app>
    <v-app-bar app>
      <v-toolbar-title class="headline text-uppercase">
        <span>TensorFlow </span>
        <span class="font-weight-light">IMAGE CLASSIFIER</span>
      </v-toolbar-title>
      <v-spacer></v-spacer>
      <v-btn
        text
        href="https://github.com/tigerwill90/tfjs-vuetify-image-classifier"
        target="_blank"
      >
        <span class="mr-2">Github</span>
      </v-btn>
    </v-app-bar>

    <v-content>
      <Classifier/>
    </v-content>
  </v-app>
</template>

<script>
import Classifier from './components/Classifier'

export default {
  name: 'App',
  components: {
    Classifier,
  },
  data: () => ({
    //
  }),
}
</script>
