<template>
  <v-container>
        <v-content> 
            <v-card width="auto" class="mx-auto mt-5"> 
            <v-row align="center"
              justify="space-around"
              class="mb-5 mt-5">
              <div class="about">
                <h1 class="font-italic font-weight-bold">Sistema ESID</h1>
              </div>
            </v-row>
            <div class="home ma-10" color="orange-darken-3">
              <v-carousel show-arrows="hover">
                <v-carousel-item
                  :src="require('../assets/esid1.png')"
                  aspect-ratio="16/9"
                ></v-carousel-item>

                <v-carousel-item
                  :src="require('../assets/esid2.png')"
                  aspect-ratio="16/9"
                ></v-carousel-item>

                <v-carousel-item
                  :src="require('../assets/esid3.png')"
                  aspect-ratio="16/9"
                ></v-carousel-item>
              </v-carousel>
            </div>
            </v-card> 
        </v-content>    
    </v-container>
</template>

<script>
// @ is an alias to /src
//import HelloWorld from '@/components/HelloWorld.vue'

export default {
  name: 'HomeView',
  components: {
    //HelloWorld
  }
}
</script>
