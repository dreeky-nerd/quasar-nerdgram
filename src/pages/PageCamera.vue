<template>
  <q-page class="constrain-more q-pa-md">
    <div class="camera-frame q-pa-md">
      <video ref="video" class="full-width" autoplay v-show="!imageCaptured"/>
      <canvas
        ref="canvas"
        class="full-width"
        height="240"
        v-show="imageCaptured"
      />
    </div>
    <div class="text-center q-pa-md">
      <q-btn
        v-if="hasCameraSupport"
        icon="eva-camera"
        round
        color="grey-10"
        size="lg"
        @click="captureImage"/>
      <q-file
        v-else
        v-model="imageUpload"
        accept="image/*"
        label="Choose as image"
        outlined
        @input="captureImageFallback"
      >
        <template v-slot:prepend>
          <q-icon name="eva-attach-outline"/>
        </template>
      </q-file>
      <div class="row justify-center q-ma-md">
        <q-input class="col col-sm-6" label="caption" dense/>
      </div>
      <div class="row justify-center q-ma-md">
        <q-input class="col col-sm-6" label="location" dense>
          <template v-slot:append>
            <q-btn icon="eva-navigation-2-outline"
            dense
            flat
            round/>
          </template>
        </q-input>
      </div>
      <div class="row justify-center q-mt-lg">
        <q-btn
          color="primary"
          label="post Image"
          rounded
          unelevated
        />
      </div>
    </div>
  </q-page>
</template>

<script>
import { uid } from 'quasar';

require('md-gum-polyfill');

export default {
  name: 'PageCamera',
  data() {
    return {
      post: {
        id: uid(),
        caption: '',
        location: '',
        photo: null,
        date: Date.now(),
      },
      imageCaptured: false,
      hasCameraSupport: true,
      imageUpload: [],
    };
  },
  methods: {
    initCamera() {
      navigator.mediaDevices.getUserMedia({
        video: true,
      }).then((stream) => {
        this.$refs.video.srcObject = stream;
        // eslint-disable-next-line no-unused-vars
      }).catch((error) => {
        this.hasCameraSupport = false;
      });
    },
    captureImage() {
      let video = this.$refs.video;
      let canvas = this.$refs.canvas;
      canvas.width = video.getBoundingClientRect().width;
      canvas.height = video.getBoundingClientRect().height;
      let context = canvas.getContext('2d');
      context.drawImage(video, 0, 0, canvas.width, canvas.height);
      this.imageCaptured = true;
      this.post.photo = this.dataURItoBlob(canvas.toDataURL());
    },
    captureImageFallback(file) {
      this.post.photo = file;
      let reader = new FileReader();
      let canvas = this.$refs.canvas;
      let context = canvas.getContext('2d');
      reader.onload = (event) => {
        let img = new Image();
        img.onload = () => {
          canvas.width = img.width;
          canvas.height = img.height;
          context.drawImage(img, 0, 0, canvas.width, canvas.height);
          this.imageCaptured = true;
        };
        img.src = event.target.result;
      };
      reader.readAsDataURL(file);
    },
    dataURItoBlob(dataURI) {
      // convert base64 to raw binary data held in a string
      // doesn't handle URLEncoded DataURIs - see SO answer #6850276 for code that does this
      let byteString = atob(dataURI.split(',')[1]);
      // separate out the mime component
      let mimeString = dataURI.split(',')[0].split(':')[1].split(';')[0];
      // write the bytes of the string to an ArrayBuffer
      let ab = new ArrayBuffer(byteString.length);
      // create a view into the buffer
      let ia = new Uint8Array(ab);
      // set the bytes of the buffer to the correct values
      for (let i = 0; i < byteString.length;) {
        ia[i] = byteString.charCodeAt(i);
        i += 1;
      }
      // write the ArrayBuffer to a blob, and you're done
      let blob = new Blob([ab], { type: mimeString });
      return blob;
    },
  },
  mounted() {
    this.initCamera();
  },
};
</script>

<style lang="sass">
  .camera-frame
    border: 2px solid $grey-10
    border-radius: 10px
</style>
