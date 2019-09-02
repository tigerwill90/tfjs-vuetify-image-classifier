<template>
  <v-container>
    <v-layout
      text-center
      wrap
    >
      <v-flex xs12>
        <v-img
          :src="require('../assets/tf.png')"
          class="my-3"
          contain
          height="200"
        ></v-img>
      </v-flex>

      <v-flex mb-4>
        <h1 class="display-2 font-weight-bold mb-3">
          Welcome to my first tfjs <br> image classifier
        </h1>
        <p class="subheading font-weight-regular">
          Choose an image and we will try to find a match
        </p>
      </v-flex>

      <!-- TODO add loading animation (waiting for model) -->
      <v-flex
        v-if="net"
        mb-5
        xs12
      >
        <v-layout justify-center>
          <v-file-input
                  style="max-width: 260px"
                  :rules="rules"
                  accept="image/png, image/jpeg, image/bmp, image/jpg"
                  placeholder="Pick an image"
                  prepend-icon="mdi-camera"
                  show-size
                  flat
                  @change="selectFile"
          ></v-file-input>
        </v-layout>
      </v-flex>

      <v-flex mb-5 xs12>
        <v-layout justify-center>
          <v-img v-if="img" :src="img.src" max-width="227" max-height="227">
            <template v-slot:placeholder>
              <v-row
                      class="fill-height ma-0"
                      align="center"
                      justify="center"
              >
                <v-progress-circular indeterminate color="grey lighten-5"></v-progress-circular>
              </v-row>
            </template>
          </v-img>
        </v-layout>
      </v-flex>

      <v-flex v-if="results" mb-5 xs12>
        <h2>Result</h2>
        <v-layout justify-center>
          <v-simple-table fixed-header height="300px">
            <thead>
            <tr>
              <th class="text-left">Class name</th>
              <th class="text-left">Probability</th>
            </tr>
            </thead>
            <tbody>
            <tr v-for="(result, i) in results" :key="i">
              <td style="text-align: left">{{ result.className }}</td>
              <td style="text-align: left">{{ result.probability }}</td>
            </tr>
            </tbody>
          </v-simple-table>
        </v-layout>
      </v-flex>

    </v-layout>
  </v-container>
</template>

<script>
import * as mobilenet from '@tensorflow-models/mobilenet'
export default {
  title: 'Classifier',
  data: () => ({
    rules : [
      value => !value || value.size < 2000000 || 'Image size should be less than 2 MB!',
    ],
    net: null,
    img: null,
    results: null,
  }),
  methods: {
    selectFile(file) {
      // if file exist, display and classify image
      if (file) {
        const reader = new FileReader()
        reader.readAsDataURL(file)

        // create image onload
        reader.onload = res => {
          this.img = new Image()
          this.img.src = res.target.result
          this.img.height = 227
          this.img.width = 227
        }

        // catch loadend event and classify with mobilenet
        reader.addEventListener('loadend', () => {
          this.net.classify(this.img).then(res => {
            this.results = res
          }).catch(err => {
            alert(err.message)
          })
        })
      } else {
        this.img = null
        this.results = null
      }
    }
  },
  mounted() {
    mobilenet.load().then(res => {
      this.net = res
      console.warn('sucessfully loaded model')
    }).catch(err => {
      alert(err.message)
    })
  }
}
</script>
