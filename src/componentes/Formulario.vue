<template lang="html">

  <div class="jumbotron">
    <h2>Formulario Estudiante</h2>
    
    <vue-form :state="formState" @submit.prevent="enviar()">

      <validate tag="div">
        <label for="nombre">Nombre</label>
        <input class="form-control" 
               type="text" 
               id="nombre" 
               name="nombre" 
               v-model.trim="formData.nombre"
               autocomplete="off" 
               :minlength="nombreMin" 
               :maxlength="nombreMax" 
               no-espacios
               required
        >
        <field-messages name="nombre" show="$dirty">
          <div slot="required" class="alert alert-dark mt-1">Campo requerido</div>
          <div slot="no-espacios" class="alert alert-dark mt-1">No se permiten espacios</div>
          <div slot="minlength" class="alert alert-dark mt-1">Este campo requiere un nombre de minimo {{ nombreMin }} letras </div>
          <div v-if="formData.nombre.length == nombreMax" class="alert alert-dark mt-1">El nombre debe tener como máximo {{ nombreMax }} caracteres </div>
        </field-messages>
      </validate>

      <validate tag="div">
        <label for="apellido">Apellido</label>
        <input class="form-control" 
               type="text" 
               id="apellido" 
               name="apellido" 
               v-model.trim="formData.apellido"
               autocomplete="off"  
               :minlength="nombreMin"
               :maxlength="nombreMax"  
               no-espacios
               required
        >
        <field-messages name="apellido" show="$dirty">
          <div slot="required" class="alert alert-dark mt-1">Campo requerido</div>
          <div slot="no-espacios" class="alert alert-dark mt-1">No se permiten espacios</div>
          <div slot="minlength" class="alert alert-dark mt-1">Este campo requiere un nombre de minimo {{ nombreMin }} letras </div>
          <div v-if="formData.apellido.length == nombreMax" class="alert alert-dark mt-1">El apellido debe tener como máximo {{ nombreMax }} caracteres </div>
        </field-messages>
      </validate>

      <validate tag="div">
        <label for="nota">Nota</label>
        <input class="form-control" 
               type="number" 
               id="nota" 
               name="nota"
               v-model.number="formData.nota"
               autocomplete="off" 
               :min="notaMin" 
               :max="notaMax"
               required
               >
        <field-messages name="nota" show="$dirty">
          <div slot="required" class="alert alert-dark mt-1">Campo requerido</div>
          <div slot="min" class="alert alert-dark mt-1">La nota no puede ser menor a {{ notaMin }}</div>
          <div slot="max" class="alert alert-dark mt-1">La nota no puede ser mayor a {{ notaMax }}</div>
        </field-messages>
      </validate>

      <button class="btn btn-success my-3" :disabled="formState.$invalid" type="submit">Enviar</button>
    </vue-form>

    <div v-if="notas.length" class="table-responsive">
      <table class="table table-dark">
        <tr>
          <th> Estudiante </th>
          <th> Nota </th>
        </tr>
        <tr v-for="(pers, i) in notas" :key="i">
          <td>{{ pers.nombre }} {{ pers.apellido }}</td>
          <td v-if="pers.nota >= 0 && pers.nota <= 3" class="alert alert-danger"> {{ pers.nota }} </td>
          <td v-else-if="pers.nota >= 4 && pers.nota <= 6" class="alert alert-warning"> {{ pers.nota }} </td>
          <td v-else class="alert alert-success"> {{ pers.nota }} </td>
        </tr>
        <tr> Promedio 
          <td v-if="promedio >= 0 && promedio <= 3" class="alert alert-danger"> {{ promedio }} </td>
          <td v-else-if="promedio >= 4 && promedio <= 6" class="alert alert-warning"> {{ promedio }} </td>
          <td v-else class="alert alert-success"> {{ promedio }} </td>
        </tr>  
      </table>
    </div>
    <h3 class="alert alert-danger" v-else>¡No se encontraron notas!</h3>
  </div>

</template>

<script lang="js">

export default {
  name: 'src-components-formulario-nota',
  components: {},
  props: [],
  data() {
    return {
      formData: this.getFormInicial(),
      formState: {},
      nombreMin: 4,
      nombreMax: 15,
      notaMin: 0,
      notaMax: 10,
      notas: [],
      promedio: 0,
      acumNotas: 0,
    }
  },
  computed: {},
  mounted() {},
  methods: {
    getFormInicial() {
      return {
        nombre: '',
        apellido: '',
        nota: '',
      }
    },
    enviar() {
      let note = this.formData
      this.notas.push(note)
      this.acumNotas += note.nota
      this.promedio = this.acumNotas / this.notas.length
      this.formData = this.getFormInicial()
      this.formState._reset()
    }
  }
}
</script>

<style scoped lang="css">
.jumbotron {
  background-color: rgb(9, 9, 128);
  color: white;
}
hr {
  background-color: #eee;
}
</style>