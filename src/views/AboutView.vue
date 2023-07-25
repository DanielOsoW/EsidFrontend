<template>
  <v-container>
    <v-row align="center"
            class="pa-2">
     <v-btn
      color="light-green"
      class="ml-1 mt-5"
      @click="exportar()"
    >
      Descargar Datos (XLSX)
    </v-btn>
  </v-row>
  <br/>
  <br/>
  <v-row >
          <div class="about">
            <h1>Trámites realizados</h1>
          </div>
  </v-row>
  <v-col align="justify-center" class="mt-3">
    <v-row align="center"
            class="pa-2"
            v-for="(item, i) in items"
            :key="i">
            <v-container>
              <v-card
              class="pa-2" 
              color="light-green-darken-1"
              dark
            >
              <v-card-title class="text-h5 text-black" >
                {{item.id}}. {{item.titulo}}
              </v-card-title>

              <v-card-subtitle class="text-h6 text-white">{{item.enunciado}}</v-card-subtitle>

              <br>
              
              <v-card-actions>
                  <v-btn variant="outlined" color="black" @click="goToData(item.id)">
                    Visualizar Datos
                  </v-btn>
              </v-card-actions>
            </v-card>
            </v-container>
            
    </v-row>
  </v-col>  
  </v-container>
</template>
  
<script>
  export default {
      data: () => ({
        items:[
          {'id':1,'titulo':'Longa','enunciado':'longueiro'},
          {'id':2,'titulo':'Longo','enunciado':'longueira'},
          {'id':3,'titulo':'Longe','enunciado':'longueire'}
        ],
        citems:[
          'Normal','Urgente','Anticipado'
        ],
        gitems:[
          'Solicitud','Reclamo','Sugerencia','Felicitaciones'
        ],
        titems:[
          'Proyección','Aire','Limpieza','Espacio'
        ],
          title: '',
          snackbar: false,
          text: `Debes ingresar los datos requeridos`,
          nombres:null,
          apellido1:null,
          apellido2:null,
          correo:null,
          nueva1: null,
          nueva2: null,
          roles:[],
          rol: null,
          tramite: null,
          caracter: null,
          tipossolicitud: null,
          idrol:null,
          carreras:[],
          carrera:null,
          idcarrera:null,
          edad: null,
          sexo:"",
          anos_experiencia:null,
          cantiCarreras: null,
          items2: [],
          showPassword1:false,
          showPassword2:false,
          valid: true,
          name: '',
          checkbox: false,
          carCheck: false,
          usuario: null,
          texto1: null,
          sexos: [
          'Femenino',
          'Masculino',
          'Prefiero no decirlo'
        ],
        nameRules: [
            v => !!v || 'Se requiere su nombre',
            v => (v && v.length <= 40) || 'El nombre no puede sobrepasar los 40 caracteres',
          ],
          email: '',
          emailRules: [
            v => !!v || 'Se requiere su E-mail',
            v => /.+@.+\..+/.test(v) || 'El E-mail proporcionado debe ser válido',
          ],
          apellidoRules: [
            v => !!v || 'Se requiere el apellido',
            v => (v && v.length <= 200) || 'El apellido no puede contener más de 200 caracteres',
          ],
          nuevaRules: [
            v => !!v || 'Debes ingresar la nueva contraseña',
            v => (v && v.length >= 12) || 'La contraseña debe contener más de 12 caracteres',
          ],
          edadRules: [
            v => !!v || 'Edad requerida',
            v => (v && v > 10 && v < 100) || 'Edad inválida',
          ],
          sexoRules: [
            v => !!v || 'Sexo requerido',
            v => (v != null) || 'Sexo requerido',
          ],
          entidadRules: [
            v => !!v || 'Entidad requerida',
            v => (v != null) || 'Entidad requerida',
          ],
          tituloRules: [
            v => !!v || 'Título requerido',
            v => (v != null) || 'Título requerido',
          ],
          expRules: [
          v => !!v || 'Experiencia requerida',
          v => (v && v > -1) || 'Experiencia inválida',
          ],
      }),
  
      methods: {
        validate () {
          this.$refs.form.validate()
        },
        reset () {
          this.$refs.form.reset()
        },
        resetValidation () {
          this.$refs.form.resetValidation()
        },
  
        //Función asíncrona para consultar los datos
      getData: async function(){
          try {
              let response = await this.$http.get('carreras/all');
              this.carreras = response.data;
              this.cantiCarreras = this.carreras.length;
              for(var i = 0;i<this.cantiCarreras;i++){
                  var nombre = (this.carreras[i].nombre_carrera)+"";
                  this.items[i] = nombre;
              }
              var result2 = await this.$http.get('roles/all');
              let response2 = result2.data;
              this.roles = response2;
              for(var j = 1;j<this.roles.length;j++){
                  var nombre2 = (this.roles[j].nombre_rol)+"";
                  this.items2[j] = nombre2;
              }
  
              this.carCheck = true;
              } catch (error) {
                  console.log('error', error);
              }
      },
  
  
      register: async function() {
          this.snackbar = false;
          if(this.nueva1!=this.nueva2){
              this.text = "Las contraseñas son distintas"
              this.snackbar = true;
          }
          else if(this.nombres != '' && this.paterno!='' && this.materno != '' &&  this.correo != '' && this.nueva1!='' && this.nueva2!='' &&  this.carrera != ''  &&  this.rol != '' && this.anos_experiencia!=null && this.edad!=null && this.sexo!=''){
              try {
                  for(var j=0;j<this.carreras.length;j++){
                    if(this.carrera==this.carreras[j].nombre_carrera){
                      this.idcarrera = this.carreras[j].id;
                    }
                  }
                  for(var k=0;k<this.roles.length;k++){
                    if(this.rol==this.roles[k].nombre_rol){
                      this.idrol = this.roles[k].id;
                    }
                  }
                  console.log(this.idcarrera,this.idrol);
                  var result = await this.$http.post('usuarios/create',{"apellido1":this.apellido1, "apellido2":this.apellido2, "nombres":this.nombres, "correo":this.correo, "password":this.nueva1, "rol":this.idrol, "carrera":this.idcarrera, "anos_experiencia":this.anos_experiencia,"edad":this.edad, "sexo":this.sexo});
                  let response2 = result.data;
                  this.usuario = response2.data;
                  this.texto1='Información editada con éxito';  
                  this.$router.push('/login')
              }catch (error) {
                  console.log('error', error);
              }
          }
          else{
              this.text='El usuario no se ha podido crear. Ingrese la información requerida';
              this.snackbar = true;
          }
            
      },
      
      goToProgress(item) {
          const enunciadoID = item;
          window.location.href = "http://"+enunciadoID;
      },

      },
      //Función que se ejecuta al cargar el componente
      created:function(){
          this.getData();
      }
  }
</script>