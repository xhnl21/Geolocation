<template>
  <ion-page>
    <ion-header :translucent="true">
      <ion-toolbar>
        <ion-title>Blank</ion-title>
      </ion-toolbar>
    </ion-header>

    <ion-content :fullscreen="true">
      <ion-header collapse="condense">
        <ion-toolbar>
          <ion-title size="large">Blank</ion-title>
        </ion-toolbar>
      </ion-header>

      <div id="container">
          lat: {{ lat }}
          <br>
          lng: {{ lng }}
          <br>
          <button @click="track">
            Get Current LocationA
          </button> 
          <br><br><br>
          <button @click="clearWatch(false)">
            Stop Current LocationA
          </button> 
      </div>
    </ion-content>
  </ion-page>
</template>

<script>

// import { defineComponent, ref } from 'vue';
import { Geolocation } from '@capacitor/geolocation';
export default {
  data() {
    return {
      loc: {
        lat:0,
        long:0
      },
      coord:[],
      coordA:[],
      coordB:[],
      loadingPosition: false,
      geolocation: {},
      lat:0,
      lng:0,
      position:[],
      location:[],
      marker:[],
      watchId:null,
      bool:true,
    }
  },
  methods:{
    async track() {
      // this.location = await Geolocation.getCurrentPosition({
      //     enableHighAccuracy: true,
      //     timeout: 1000,
      //     maximumAge: 1000,
      // });
      if (this.bool !== false) {
        this.watchId = await Geolocation.watchPosition({
            enableHighAccuracy: true,
            timeout: 1000,
            maximumAge: 1000,
          }, (position, err) => {
            console.log(err);
            if (err !== undefined) {
                this.clearWatch();
                return;
            }
            console.log("----->Watch latitude" + position.coords.latitude);
            console.log("-----> Watch logitude" + position.coords.longitude);
            console.log("-----> Watch accuracy" + position.coords.accuracy);
            console.log(this.time());
            this.lat = position.coords.latitude
            this.lng = position.coords.longitude
        })
        console.log(this.watchId);
      }
    },
    clearWatch() {
      // if (this.bool) {
        if (this.watchId != null) {
          console.log(this.watchId);
            Geolocation.clearWatch({ id: this.watchId });
            this.track()
        } else {
          this.track()
        }
      // } 
      // else {
      //   Geolocation.clearWatch({ id: this.watchId });
      // }
    },
    async stopTrack() {
      const opt = {id: await this.watchId};
      Geolocation.clearWatch(opt).then(result=>{
        console.log('result of clear is',result);
      });
    },
    time() {
        const actual = new Date();
        const hora = actual.getHours();
        const minuto = actual.getMinutes();
        const segundo = actual.getSeconds();
        return hora + " : " + minuto + " : " + segundo;
    }
  },
  watch: {
    coord: {
      handler(val, oldVal) {
        this.coordB = val;
        console.log(val, oldVal)
      },
      deep: true
    },
  },
  setup() {
    return { 
      // IonContent, IonHeader, IonPage, IonToolbar, IonTitle
    };
  }
}
// export default defineComponent({
//   setup() {
//     const loc = ref<{
//       lat: null | number;
//       long: null | number;
//     }>({
//       lat: null,
//       long: null,
//     });

//     const getCurrentPosition = async () => {
//       const pos = await Geolocation.getCurrentPosition({
//         enableHighAccuracy: true,
//         timeout: 30000
//       });
//       loc.value = {
//         lat: pos.coords.latitude,
//         long: pos.coords.longitude,
//       };
//     };
//     return { 
//       getCurrentPosition, 
//       loc,
//       IonContent, IonHeader, IonPage, IonTitle, IonToolbar
//     };
//   },
// });
</script>

<style scoped>
#container {
  text-align: center;
  
  position: absolute;
  left: 0;
  right: 0;
  top: 50%;
  transform: translateY(-50%);
}

#container strong {
  font-size: 20px;
  line-height: 26px;
}

#container p {
  font-size: 16px;
  line-height: 22px;
  
  color: #8c8c8c;
  
  margin: 0;
}

#container a {
  text-decoration: none;
}
</style>
