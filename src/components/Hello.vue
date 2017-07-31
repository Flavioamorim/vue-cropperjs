<template>
    <div>


            <input type="file" name="image" accept="image/*" style="font-size: 1.2em; padding: 10px 0;" v-on:change="setImage"/>
            <br />
            <div>
                <vue-cropper
                    ref='cropper'
                    :guides=true
                    :view-mode=2
                    drag-mode="crop"
                    :auto-crop-area=0.5
                    :min-container-width=250
                    :min-container-height=180
                    :background=true
                    :rotatable=true
                    :aspect-ratio=15/9
                    :src="imgSrc"
                    alt="text"
                    :img-style="{ width: '400px', height: '300px' }"
                    :cropmove="cropImage">
                </vue-cropper>
            </div>
            <br>
                <img
                    :src="cropImg"
                    style="width: 1043px; height: 197px; border: 1px solid gray"
                    alt="Cropped Image"/>

                  <br>

                  <button type="button" @click="save()">save img </button>



    </div>
</template>

<script>

import VueCropper from 'vue-cropperjs';

export default {
  name: 'hello',
  components: {
      VueCropper
  },
  data () {
    return {
      msg: 'Welcome to Your Vue.js App',
      existImage:false,
      imgSrc: '',
      cropImg: null
    }
  },
  methods:{
      save(){

          console.log(this.cropImg)

      },
      setImage (e) {
          const file = e.target.files[0];

          if (!file.type.includes('image/')) {
              alert('Please select an image file');
              return;
          }

          if (typeof FileReader === 'function') {
              const reader = new FileReader();

              reader.onload = (event) => {
                  this.imgSrc = event.target.result;
                  // rebuild cropperjs with the updated source
                  this.$refs.cropper.replace(event.target.result);
              };

              reader.readAsDataURL(file);
              console.log(this.cropImg)
          } else {
              alert('Sorry, FileReader API not supported');
          }
      },
      cropImage () {
          // get image data for post processing, e.g. upload or setting image src
          this.cropImg = this.$refs.cropper.getCroppedCanvas().toDataURL();

      },
      rotate () {
          // guess what this does :)
          this.$refs.cropper.rotate(90);
      },
      imageuploaded(res) {

          console.log(res)

      }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h1, h2 {
  font-weight: normal;
}

ul {
  list-style-type: none;
  padding: 0;
}

li {
  display: inline-block;
  margin: 0 10px;
}

a {
  color: #42b983;
}
</style>
