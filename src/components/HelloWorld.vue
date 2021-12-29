<template>

<v-app>
            <v-btn class="pa-10" x-large color="success" type="submit" @click="dialog=true" >Add thing to do</v-btn>
    <v-container grid-list-xl>

    <v-row row wrap>
        

        <v-col md="12">
            <v-card class="mb-3" v-for="(item, index) in listaTareas" :key="index">
<v-card-text>
    <v-chip
      
      color="blue"
      label
      text-color="white"
      class="ml-0"
    >
      <v-icon left>
        mdi-label
      </v-icon>
{{item.titulo}}
    </v-chip>
<p class="mt-2">{{item.descripcion}}</p>
    <v-btn fab small color="warning" class="ml-0" @click="editar(index)"><v-icon>
        mdi-square-edit-outline
      </v-icon></v-btn>
    <v-btn fab small color="error" class="ma-1" @click="eliminarTarea(item.id)">    <v-icon>
        mdi-delete
      </v-icon></v-btn>

</v-card-text>
            </v-card>
        </v-col>


    </v-row>
    <v-snackbar
      v-model="snackbar"
    >
      {{ mensaje }}

      <template v-slot:action="{ attrs }">
        <v-btn
          color="red"
          text
          v-bind="attrs"
          @click="snackbar = false"
        >
          Cerrar
        </v-btn>
      </template>
    </v-snackbar>
        </v-container>

        <v-dialog v-model="dialog" max-width=80%>
    <v-card >
        <v-main>
               <v-form  @submit.prevent="agregarTarea">
                  <v-card-text><v-text-field label="Task title" v-model="titulo" ></v-text-field>
                   <v-textarea label="Description of the task" v-model="descripcion"></v-textarea>
                                <v-combobox
                                v-model="chips"
                                :items="items"
                                chips 
                                clearable
                                label="Your favorite hobbies"
                                multiple
                                solo
                            >
                                <template v-slot:selection="{ attrs, item, select, selected }">
                                <v-chip
                                    v-bind="attrs"
                                    :input-value="selected"
                                    close
                                    @click="select"
                                    @click:close="remove(item)"
                                >
                                    <strong>{{ item }}</strong>&nbsp;
                                </v-chip>
                                </template>
                            </v-combobox>
                   <v-btn large color="success" type="submit" @click.stop="dialog = false" >Add thing to do</v-btn>

                   </v-card-text>
               </v-form>

        </v-main>
    </v-card>
</v-dialog >
<v-dialog v-model="edit" max-width=50% persistent>
        
            <v-card class="pa-3">
               <v-form @submit.prevent="editarTarea">
                   <v-text-field label="Task title" v-model="titulo"></v-text-field>
                   <v-textarea label="Description of the task" v-model="descripcion"></v-textarea>
                   <v-row justify="space-around" class="pa-4">
                   <v-btn large color="warning" type="submit" @click.stop="edit = false">Edit</v-btn>
                   <v-btn large color="error" @click="closeEdit()">Salir</v-btn>
                   </v-row>
               </v-form>
            </v-card>
            </v-dialog>
</v-app>

</template>



<script>
export default {
    data() {
       return{
           listaTareas:[
               {id:1, titulo:'Thing to do #1', descripcion: 'Lorem ipsum, dolor sit amet consectetur adipisicing elit. Harum, laborum!'},
               {id:2, titulo:'Thing to do #2', descripcion: 'Lorem ipsum, dolor sit amet consectetur adipisicing elit. Harum, laborum!'},
               {id:3, titulo:'Thing to do #3', descripcion: 'Lorem ipsum, dolor sit amet consectetur adipisicing elit. Harum, laborum!'}
           ],
       titulo:"",
       descripcion:"",
       snackbar:false,
       mensaje: "",
       indexTarea:"",
       dialog:false,
       edit:false,
        chips: ['Programming', 'Playing video games', 'Watching movies', 'Sleeping'],
        items: ['Streaming', 'Eating'],
       }
    },
    methods:{
        agregarTarea(){
           // console.log(this.titulo, this.descripcion);
            if(this.titulo ==='' || this.descripcion === ''){
                this.snackbar=true
                this.mensaje= "Fill in all the fields to add task"
            } else{
                this.listaTareas.unshift({
                    id:Date.now(),
                    titulo: this.titulo,
                    descripcion:this.descripcion
                })
                this.titulo=''
                this.descripcion=''
                this.snackbar=true,
                this.mensaje='Added Task'
                }
        },
        eliminarTarea(id){
            this.listaTareas = this.listaTareas.filter(e => e.id != id)
        },
        closeEdit(){
            this.edit=false
            this.snackbar=false
            this.titulo=""
            this.descripcion=""
        },
        editar(index){
            this.edit=true
            this.titulo = this.listaTareas[index].titulo
            this.descripcion= this.listaTareas[index].descripcion
            this.indexTarea = index
        },
        editarTarea(){
            this.listaTareas[this.indexTarea].titulo=this.titulo
            this.listaTareas[this.indexTarea].descripcion=this.descripcion
            this.titulo=""
            this.descripcion=""
                this.snackbar=true
                this.mensaje='You edited the task'
        },
         remove (item) {
        this.chips.splice(this.chips.indexOf(item), 1)
        this.chips = [...this.chips]
      }
    }
}
</script>