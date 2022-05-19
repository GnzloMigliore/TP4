<template>
  <section class="src-components-formulario">
    <div class="jumbotron">
      <vue-form :state="formState" @submit.prevent="enviar()">
        <!-- Campo nombre -->
        <validate tag="div">
          <!-- elemento de entrada -->
          <label for="nombre">Nombre</label>
          <input
            type="text"
            id="nombre"
            name="nombre"
            class="form-control"
            v-model.trim="formData.nombre"
            autocomplete="off"
            required
            :minlength="nombreMinLength"
            :maxlength="nombreMaxLength"
            no-espacios
          />

          <!-- Mensajes de validacion -->
          <field-messages name="nombre" show="$dirty">
            <div slot="required" class="alert alert-danger mt-1">
              Campo requerido
            </div>
            <div slot="minlength" class="alert alert-danger mt-1">
              Este campo requiere entre {{ nombreMinLength }} y
              {{ nombreMaxLength }} caracteres
            </div>
            <div slot="no-espacios" class="alert alert-danger mt-1">
              No se permite espacios intermedios en este campo
            </div>
          </field-messages>
        </validate>

        <!-- Campo edad -->
        <validate tag="div">
          <!-- elemento de entrada -->
          <label for="edad">Edad</label>
          <input
            type="number"
            id="edad"
            name="edad"
            class="form-control"
            v-model.trim="formData.edad"
            autocomplete="off"
            required
            :min="edadMin"
            :max="edadMax"
          />

          <!-- Mensajes de validacion -->
          <field-messages name="edad" show="$dirty">
            <div slot="required" class="alert alert-danger mt-1">
              Campo requerido
            </div>
            <div slot="minlength" class="alert alert-danger mt-1">
              Este campo requiere como minumo {{ nombreMinLength }} caracteres
            </div>
            <div slot="min" class="alert alert-danger mt-1">
              La edad minima permitida es de {{ edadMin }}
            </div>
            <div slot="max" class="alert alert-danger mt-1">
              La edad maxima permitida es de {{ edadMax }}
            </div>
          </field-messages>
        </validate>

        <!-- Campo email -->
        <validate tag="div">
          <!-- elemento de entrada -->
          <label for="email">Email</label>
          <input
            type="email"
            id="email"
            name="email"
            class="form-control"
            v-model.trim="formData.email"
            autocomplete="off"
            required
          />

          <!-- Mensajes de validacion -->
          <field-messages name="email" show="$dirty">
            <div slot="required" class="alert alert-danger mt-1">
              Campo requerido
            </div>
            <div slot="email" class="alert alert-danger mt-1">
              Email no valido
            </div>
          </field-messages>
        </validate>
        
        <!-- Botón de envío -->
      <button
          type="submit"
          class="btn btn-dark my-4"
          :disabled="formState.$invalid"
        >
          Enviar
        </button>
      </vue-form>

      <table class="table">
        <thead class="thead-dark">
          <tr>
            <th scope="col">Nombre</th>
            <th scope="col">Edad</th>
            <th scope="col">Email</th>
          </tr>
        </thead>
        <tbody>
            <tr v-for="(user,index) in users" :key="index">
            <td>{{ user.nombre }}</td>
            <td>{{ user.edad }}</td>
            <td>{{ user.email }}</td>
          </tr>
        </tbody>
      </table>
    </div>
  </section>
</template>

<script>
export default {
  name: "src-components-formulario",
  props: [],
  mounted() {},
  data() {
    return {
      formData: this.getInicialData(),
      formState: {},
      nombreMinLength: 5,
      nombreMaxLength: 15,
      edadMin: 18,
      edadMax: 120,
      isDisabled: false,
      users: []
    };
  },
  methods: {
    getInicialData() {
      return {
        nombre: "",
        edad: "",
        email: "",
      };
    },
    enviar() {
      console.log({...this.formData});
      this.users.push({...this.formData})
      this.formData = this.getInicialData(); //reset de los datos del vue-form
      this.formState._reset(); //reseet de los estados vue-form
    },
  },
  computed: {},
  isDisabled:function(){

  },
};
</script>

<style scoped lang="css"></style>
