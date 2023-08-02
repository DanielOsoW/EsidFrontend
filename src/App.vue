<template>
  
  <v-app class="#app">
    <v-toolbar color="grey-lighter-8" :elevation="8" >
  
            <v-app-bar-nav-icon color="black" @click.stop="drawer = !drawer">
        
      </v-app-bar-nav-icon>
      
      <v-img
          alt="FING Logo"
          class="d-flex shrink"
          contain
          :src="require('./assets/fing.png')"
          transition="scale-transition"
          width="-2"
          @click="home()"
        />
        

      <v-toolbar-title class="d-flex show font-italic font-weight-bold">Edificio Salas de Innovación Docente</v-toolbar-title>

      <v-spacer></v-spacer>
      <v-spacer></v-spacer>
      

            <v-btn variant="outlined" class="primary mr-2" color="teal-lighten-1" @click="login()">
              <v-icon icon="mdi-account-arrow-right" class="mr-2"> 
              </v-icon>Ingresar
            </v-btn>

            <v-btn variant="outlined" class="primary" color="orange-darken-2" @click="register()">
              <v-icon icon="mdi-account-plus" class="mr-2"> 
              </v-icon>Registrarme
            </v-btn>

    </v-toolbar>
  <router-view/>
  </v-app>
  <v-layout>
      <v-navigation-drawer
        v-model="drawer"
        temporary
        width="290"
      >
        <v-list-item
          prepend-avatar="https://randomuser.me/api/portraits/men/78.jpg"
          title="Isidro Cornejo"
        ></v-list-item>

        <v-divider></v-divider>

        <v-list density="" nav>
          <v-list-item prepend-icon="mdi-home" title="Home" value="home" @click="home()"></v-list-item>
          <v-list-item prepend-icon="mdi-account-key" title="Administrar Usuarios" value="adminusuarios" @click="adminusuarios()"></v-list-item>
          <v-list-item prepend-icon="mdi-monitor" title="Proyectores" value="proyectores" @click="proyectores()"></v-list-item>
          <v-list-item prepend-icon="mdi-monitor-lock" title="Administrar Proyectores" value="adminproyectores "></v-list-item>
          <v-list-item prepend-icon="mdi-account-card-outline" title="Enviar Solicitudes" value="solicitudes" @click="solicitudes()"></v-list-item>
          <v-list-item prepend-icon="mdi-pencil-box-multiple-outline" title="Administrar Solicitudes" value="adminsolicitudes" @click="adminsolicitudes()"></v-list-item>
          <v-list-item prepend-icon="mdi-file-document-check-outline" title="Estado Solicitudes" value="tramites" @click="tramites()"></v-list-item>
          <v-list-item prepend-icon="mdi-forum" title="Mis Notificaciones" value="notificaciones" @click="notificaciones()"></v-list-item>
          <v-list-item prepend-icon="mdi-help-circle-outline" title="Ayuda" value="ayuda" @click="ayuda()"></v-list-item>
        </v-list>
      </v-navigation-drawer>
      
    </v-layout>
</template>

<script>

import {mapGetters} from 'vuex' 

export default {
  name: 'App',
  

  components: {
  },

  methods: {

    //Función asíncrona para consultar los datos
        getData: async function(){
          try {
              this.cosas = this.$cookies.get("token");
              if(this.user==null && this.cosas!=null){

                  if(this.cosas.nombres == 'invitado'){
                    this.$store.dispatch('user',this.cosas);
                  }
                  
                  else{
                    var result2 = await this.$http.post('/user', { jwt: this.cosas.jwt});
                    let response2 = result2.data; 
                    this.$store.dispatch('user', response2);
                  }
                  
              } 
              var result = await this.$http.get('/usuarios/all');
              let response = result.data;
              this.items = response;
                
            }catch (error) {
                console.log('error', error);
            }
        },

    handleClick() { 
        this.$store.dispatch('user',null); 
        this.$router.push('/'); 
      },
    login() {
        this.$router.push('/login');
      },
    register() {
        this.$router.push('/register');
      },
    home() {
        this.$router.push('/');
      },
    proyectores() {
        this.$router.push('/proyectores');
      },
    solicitudes() {
        this.$router.push('/solicitudes');
      },
    tramites() {
        this.$router.push('/tramitessolicitudes');
      },
    notificaciones() {
        this.$router.push('/notificaciones');
      },
    ayuda() {
        this.$router.push('/ayuda');
      },
    adminusuarios() {
        this.$router.push('/adminusuarios');
      },
      adminsolicitudes() {
        this.$router.push('/adminsolicitudes');
      },
    cerrar() {
        this.$store.dispatch('user',null);
        this.$cookies.remove("token");
        this.$router.push('/');
      },
  },
  
  computed: {
    ...mapGetters(['user']) 
  },

  data: () => ({
    //
    registered: false,
    overlay: false,
    drawer: null,
    cosas:null,
    group: null,
    options: [
        {
        titulo:'Mis Datos',
        color:'primary',
        },
        {
        titulo:'Estadísticas',
        color:'secondary',
        },
        {
        titulo:'Cerrar sesión',
        color:'',
        },
      ],
  }),

   //Función que se ejecuta al cargar el componente
    created:function(){
        this.getData();
    }
};
</script>

<style scoped>
.v-application {
  background-color: #00a8a0;
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}

nav {
  padding: 30px;
}

nav a {
  font-weight: bold;
  color: #2c3e50;
}

nav a.router-link-exact-active {
  color: #42b983;
}
</style>
