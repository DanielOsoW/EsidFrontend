<template>
  <v-container>
      <v-content> 
          <v-card width="auto" class="mx-auto mt-9"> 
          <v-row align="center"
            justify="space-around"
            class="mb-5 mt-5">
            <div class="pa-2 mr-5 ml-5">
              <h1>PROYECTORES</h1>
            </div>
          </v-row>
          <v-table class="mr-6 ml-6 mb-6" theme="">
    <thead>
      <tr>
        <!--th class="text-center">
          SALA
        </th-->
        <th class="text-center">
          Administración Proyectores
        </th>
      </tr>
    </thead>
    <tbody>
      <tr
        v-for="item in desserts"
        :key="item.name"
      >
        <!--td class="text-center">{{ item.name }}</td-->
        <td class="text-center"><v-btn color="orange-darken-2" @click="goToProgress(item.calories)"><v-icon icon="mdi-remote" class=""></v-icon>{{ item.name }}</v-btn></td>
      </tr>
    </tbody>
  </v-table>
      </v-card> 
    </v-content>    
  </v-container>
</template>
  
<script>
  export default {
      data: () => ({
        desserts: [
          {
            name: 'ED101',
            calories: '10.000.148.51',
          },
          {
            name: 'ED103',
            calories: 237,
          },
          {
            name: 'ED201',
            calories: 262,
          },
          {
            name: 'ED202',
            calories: 305,
          },
          {
            name: 'ED203',
            calories: 356,
          },
          {
            name: 'ED204',
            calories: 375,
          },
          {
            name: 'ED205',
            calories: 392,
          },
          {
            name: 'ED301',
            calories: 408,
          },
          {
            name: 'ED302',
            calories: 452,
          },
          {
            name: 'ED303',
            calories: 518,
          },
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