<template>
  <div>
    <form novalidate class="md-layout" @submit.prevent="saveTrack">
      <md-card class="md-layout-item md-size-50 md-small-size-100">
        <md-card-header>
          <div class="md-title">Create track</div>
        </md-card-header>

        <md-card-content>
          <div class="md-layout md-gutter">
            <div class="md-layout-item md-small-size-100">
              <md-field :class="getValidationClass('name')">
                <label for="track-name">Track name</label>
                <md-input name="track-name" id="track-name" autocomplete="track-name" v-model="form.name" :disabled="sending" />
                <span class="md-error" v-if="!$v.form.name.required">The track name is required</span>
                <span class="md-error" v-else-if="!$v.form.name.minlength">Invalid track name</span>
              </md-field>
            </div>

            <div class="md-layout-item md-small-size-100">
              <md-field :class="getValidationClass('author')">
                <label for="track-author">Track author</label>
                <md-input name="track-author" id="track-author" autocomplete="author-name" v-model="form.author" :disabled="sending" />
                <span class="md-error" v-if="!$v.form.author.required">The author name is required</span>
                <span class="md-error" v-else-if="!$v.form.author.minlength">Invalid author name</span>
              </md-field>
            </div>
          </div>

          <div class="md-layout md-gutter">
            <div class="md-layout-item md-small-size-100">
              <md-field :class="getValidationClass('album')">
                <label for="track-album">Track album</label>
                <md-input name="track-album" id="track-album" autocomplete="album-name" v-model="form.album" :disabled="sending" />
                <span class="md-error" v-if="!$v.form.album.required">The album name is required</span>
                <span class="md-error" v-else-if="!$v.form.album.minlength">Invalid album name</span>
              </md-field>
            </div>

            <div class="md-layout-item md-small-size-100">
              <md-field :class="getValidationClass('coverImage')">
                <label for="image">Age</label>
                <md-input type="number" id="image" name="image" autocomplete="cover-image" v-model="form.coverImage" :disabled="sending" />
                <!--span class="md-error" v-if="!$v.form.coverImage.required">The image is required</span>
                <span class="md-error" v-else-if="!$v.form.coverImage.maxlength">Invalid age</span-->
              </md-field>
            </div>
          </div>

        </md-card-content>

        <md-progress-bar md-mode="indeterminate" v-if="sending" />

        <md-card-actions>
          <md-button type="submit" class="md-primary" :disabled="sending">Create track</md-button>
        </md-card-actions>
      </md-card>

      <md-snackbar :md-active.sync="trackSaved">The track {{ lastTrack }} was saved with success!</md-snackbar>
    </form>
  </div>
</template>

<script>
import { validationMixin } from 'vuelidate'
import {
  required,
  minLength
} from 'vuelidate/lib/validators'

export default {
  name: 'Create',
  mixins: [validationMixin],
  data: () => ({
    form: {
      name: null,
      author: null,
      album: null,
      coverImage: null
    },
    trackSaved: false,
    sending: false,
    lastTrack: null
  }),
  validations: {
    form: {
      name: {
        required,
        minLength: minLength(2)
      },
      author: {
        required,
        minLength: minLength(2)
      },
      album: {
        required,
        maxLength: minLength(2)
      },
      coverImage: {

      }
    }
  },
  methods: {
    getValidationClass (fieldName) {
      const field = this.$v.form[fieldName]
      if (field) {
        return {
          'md-invalid': field.$invalid && field.$dirty
        }
      }
    },
    clearForm () {
      this.$v.$reset()
      this.form.name = null
      this.form.author = null
      this.form.album = null
      this.form.coverImage = null
    },
    saveTrack () {
      const axios = require('axios')
      this.sending = true

      // Instead of this timeout, here you can call your API
      /*
      axios.post('localhost:8081/tracks', {
        name: 'Fred',
        author: 'Flintstone',
        album: 'Fred',
        coverImage: 'Flintstone'
      })
        .then(function (response) {
          console.log(response)
        })
        .catch(function (error) {
          console.log(error)
        })
        */
      axios.get('https://api.musixmatch.com/ws/1.1/chart.tracks.get?apikey=4543fec13a4b4d8b20eb049b14581080')
        .then(function (response) {
          // handle success
          console.log(response)
        })
        .catch(function (error) {
          // handle error
          console.log(error)
        })
        .then(function () {
          // always executed
        })
      this.clearForm()
    }

  }
}
</script>

<style scoped>
  .md-progress-bar {
    position: absolute;
    top: 0;
    right: 0;
    left: 0;
  }
</style>
