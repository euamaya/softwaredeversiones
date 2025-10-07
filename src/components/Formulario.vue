<template>
  <div class="container">
    <h2>Registro de Usuario</h2>
    <form @submit.prevent="validarFormulario">
      <!-- Campo Nombre -->
      <label for="nombre">Nombre:</label>
      <input
        type="text"
        id="nombre"
        v-model="nombre"
        placeholder="Ingresa tu nombre"
      />
      <span v-if="errores.nombre" class="error">{{ errores.nombre }}</span>

      <!-- Campo Edad -->
      <label for="edad">Edad:</label>
      <input
        type="number"
        id="edad"
        v-model="edad"
        placeholder="Ingresa tu edad"
      />
      <span v-if="errores.edad" class="error">{{ errores.edad }}</span>

      <!-- Campo Dirección -->
      <label for="direccion">Dirección:</label>
      <input
        type="text"
        id="direccion"
        v-model="direccion"
        placeholder="Ingresa tu dirección"
      />
      <span v-if="errores.direccion" class="error">{{ errores.direccion }}</span>

      <!-- Botón -->
      <button type="submit">Registrar</button>
    </form>

    <!-- Resultado -->
    <div v-if="mensaje" class="exito">
      {{ mensaje }}
    </div>
  </div>
</template>

<script>
export default {
  name: "Formulario",
  data() {
    return {
      nombre: "",
      edad: "",
      direccion: "",
      errores: {},
      mensaje: ""
    };
  },
  methods: {
    validarFormulario() {
      this.errores = {};
      this.mensaje = "";

      try {
        // Validar nombre
        if (!this.nombre.trim()) {
          throw { campo: "nombre", mensaje: "El nombre es obligatorio." };
        } else if (this.nombre.length < 3) {
          throw { campo: "nombre", mensaje: "Debe tener al menos 3 caracteres." };
        }

        // Validar edad
        const edadNum = parseInt(this.edad);
        if (isNaN(edadNum)) {
          throw { campo: "edad", mensaje: "La edad debe ser un número." };
        } else if (edadNum < 0 || edadNum > 120) {
          throw { campo: "edad", mensaje: "Edad fuera de rango válido." };
        }

        // Validar dirección
        if (!this.direccion.trim()) {
          throw { campo: "direccion", mensaje: "La dirección es obligatoria." };
        }

        // Si todo está bien
        this.mensaje = `Usuario registrado correctamente:
        Nombre: ${this.nombre}, Edad: ${this.edad}, Dirección: ${this.direccion}`;

        // Limpiar campos
        this.nombre = "";
        this.edad = "";
        this.direccion = "";
      } catch (error) {
        this.errores[error.campo] = error.mensaje;
      }
    }
  }
};
</script>

<style scoped>
.container {
  width: 300px;
  margin: 40px auto;
  padding: 20px;
  border-radius: 10px;
  background-color: #f4f4f4;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
}

form {
  display: flex;
  flex-direction: column;
  gap: 10px;
}

input {
  padding: 5px;
  border: 1px solid #aaa;
  border-radius: 5px;
}

button {
  background-color: #4caf50;
  color: white;
  border: none;
  padding: 8px;
  border-radius: 5px;
  cursor: pointer;
}

button:hover {
  background-color: #45a049;
}

.error {
  color: red;
  font-size: 12px;
}

.exito {
  margin-top: 15px;
  color: green;
  font-weight: bold;
  white-space: pre-line;
}
</style>
