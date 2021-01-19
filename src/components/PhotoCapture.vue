<template>
  <section :style="styling" v-if="isValid" class="photo-capture fullAreaCapture">
    <video v-show="showVideo" ref="player" class="camera" autoplay playsinline />
<!--    <figcaption>Left-Side</figcaption>-->
    <canvas v-show="!showVideo" class="preview" ref="canvas" />
    <!-- <h1>TESTING</h1> -->
    <div class="center photo-capture-actions" >
 <div  v-if="showVideo">
     <button
         :class="'btn flex-center '  + buttonsClasses"
         @click.prevent="capture"
         
     ><i class="fa fa-camera" ></i>Capture</button>
     <button
         :class="'btn flex-center '  + buttonsClasses"
         @click.prevent="changeCamera"
         
     >Switch Camera</button>
 
 </div>
      <div v-else>
          <div class="bottom-message white">


              <button :class="'btn '+ buttonsClasses" @click.prevent="cancel">{{cancelBtnContent}}</button>
              <button :class="'btn '+ buttonsClasses" @click.prevent="done">{{doneBtnContent}}</button>
          </div>
      </div>
    </div>
  </section>
</template>

<script>
export default {
  name: "PhotoCapture",
  props: {
    styling: {
      type: Object,
      isRequired: false
    },
    value: {
      default: null
    },
    hideButtons: {
      type: Boolean,
      default: false
    },
    buttonsClasses: {
      type: String,
      default: ""
    },
    captureBtnContent: {
      //default: "Capture"
    },
    cancelBtnContent: {
      default: "Cancel"
    },
    doneBtnContent: {
      default: "Done"
    },



  },
  // props: ['web_cam_open','step'],
  data() {
    return {
      showVideo: true,
      picture: null,
      isValid: true,
      isShow:false,
      stepRep: null,
      useFrontCamera: true
    };
  },
  mounted() {
    this.videoPlayer = this.$refs.player;
    this.canvasElement = this.$refs.canvas;
    //
    this.stepRep = this.step
    this.streamUserMediaVideo();

  },
  methods: {
    streamUserMediaVideo() {
      if (!navigator.mediaDevices || !navigator.mediaDevices.getUserMedia) {
        return;
      }
      navigator.mediaDevices
        .getUserMedia({ video: true ,
        facingMode : this.useFrontCamera ? "user" : "environment"
})
        .then(stream => (this.videoPlayer.srcObject = stream)).then(() => {
          this.isShow = true
        })
        .catch(() => {
          this.isValid = false;
        });
    },
    capture() {
      this.showVideo = false;
      this.canvasElement.width = this.videoPlayer.videoWidth;
      this.canvasElement.height = this.videoPlayer.videoHeight;

      var context = this.canvasElement.getContext("2d");
      context.translate(this.canvasElement.width, 0);
      context.scale(-1, 1);

      context.drawImage(this.$refs.player, 0, 0);

      this.stopVideoStream();
      this.picture = this.$refs.canvas.toDataURL();
    },
    done() {
      this.$emit("input", this.picture);
      this.showVideo = true;
      this.streamUserMediaVideo();
      

    },
    cancel() {
      this.showVideo = true;
      this.streamUserMediaVideo();
    },
    stopVideoStream() {
       console.log('128',this.$refs.player , this.$refs.player.srcObject)
      if (!(this.$refs.player && this.$refs.player.srcObject)) return;
      console.log('130',this.$refs.player , this.$refs.player.srcObject)
      this.$refs.player.srcObject.getVideoTracks().forEach(track => {
        track.stop();

      });
    },
    changeCamera() {
      alert('hi')
    this.useFrontCamera = !this.useFrontCamera;
    this.streamUserMediaVideo();
    console.log('138')
    }
  }
};
</script>

<style scoped>
video {
    /* -webkit-transform: scaleX(-1); */
    /* transform: scaleX(-1); */
  }
canvas{
  -webkit-transform: scaleX(-1);
}
</style>


