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
          <br>
          position: {{ position }}
          <br>
        <button @click="track">
            Get Current LocationA
          </button> 
          <br><br><br>
          <button @click="clearWatch(false)">
            Stop Current LocationA
          </button> 
        <!-- <div>
          <h1>Geolocation</h1>
          <p>Your location is:</p>
          <p>Latitude: {{ coord.latitude }}</p>
          <p>Longitude: {{ coord.longitude }}</p>
          <br>
          coordA: {{ coordA }}
          <br>
          coordB: {{ coordB }}
          <br>
          <button @click="getCurrentPosition">
            Get Current Location
          </button>
          <br>
          <button @click="watchPosition">
            Get Current LocationA
          </button> 
        </div>-->
      </div>
    </ion-content>
  </ion-page>
</template>

<script>
import { IonContent, IonHeader, IonPage, IonToolbar } from '@ionic/vue';

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
    trackS () {
      this.geolocation.getCurrentPosition({ enableHighAccuracy: true }).then((resp) => {
        console.log(resp);
      }).catch((error) => {
          console.log('Error getting location', error);
      });

      const watch = this.geolocation.watchPosition({ enableHighAccuracy : true, timeout: 10000 });
      watch.subscribe((data) => {
          this.data = data;
          console.log("----->Watch latitude" + data.coords.latitude);
          console.log("-----> Watch logitude" + data.coords.longitude)
          console.log("-----> Watch accuracy" + data.coords.accuracy)
      });
    },
    async track() {
      // const obj = {
      //     enableHighAccuracy: true,
      //     timeout: 1000,
      //     maximumAge: 1000,
      // };
      // this.location = await Geolocation.getCurrentPosition(obj);
      this.watchId = await Geolocation.watchPosition({
          enableHighAccuracy: true,
          timeout: 1000,
          maximumAge: 1000,
        }, (position, err) => {
          if (err !== undefined) {
              this.clearWatch();
              return;
          }
          this.lat = position.coords.latitude
          this.lng = position.coords.longitude
      })
      console.log(this.watchId);
    },
    clearWatch() {
      if (this.bool) {
        if (this.watchId != null) {
          console.log(this.watchId);
            Geolocation.clearWatch({ id: this.watchId });
            this.track()
        } else {
          this.track()
        }
      } 
      else {
        Geolocation.clearWatch({ id: this.watchId });
      }
    },
    async getCurrentPosition() {
      const coordinates = await Geolocation.getCurrentPosition();
      this.coord = coordinates;
      console.log('Current', coordinates);
    },
    async getCurrentPositionA() {
      this.loadingPosition = true;

      navigator.geolocation.getCurrentPosition(({ coords })=>{
        this.loadingPosition = false;
        this.geolocation = coords;
      });
    },
    async watchPosition() {
      const wait =  await Geolocation.watchPosition({}, (position, err) => {
        console.log(position, err);      
      })
      this.coordA = wait;
      console.log("wait", wait);
      
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
      IonContent, IonHeader, IonPage, IonToolbar
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
