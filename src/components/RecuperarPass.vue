<script setup>
import { ref } from 'vue';
import{auth} from '../firebaseConfig'
import {sendPasswordResetEmail } from "firebase/auth";

const email = ref('');
const mensaje = ref('');
const error = ref('');

// emits que vienen de la vista Home
const emit = defineEmits(['cambiarFormulario'])

// activa la funcion emit de la vista 'HomeView' para cambiar al formulario de login
const irALogin = () => {
    emit('cambiarFormulario')
}

// TODO: Implementar mensajes para cada caso
const enviarCorreo = async () => {
    try {
        const resultado = await sendPasswordResetEmail(auth, email.value)
        mensaje.value = 'Se ha enviado un correo electrónico con instrucciones para recuperar su contraseña.';
        email.value = '';
        error.value = '';
    } catch (error) {
        error.value = 'Error al tratar de enviar correo de recuperación: ' + error ;
        console.log(error)
    }
}
</script>

<template>
  <div class="container">
    <div class="row justify-content-center mt-5">
      <div class="col-md-6">
        <div class="card">
          <div class="card-header bg-primary text-white">
            <h4 class="mb-0">Recuperar Contraseña</h4>
          </div>
          <div class="card-body">
            <form @submit.prevent="enviarCorreo" class="recovery-form">
              <div class="mb-3">
                <label for="exampleInputEmail1" class="form-label">Correo Electrónico</label>
                <input type="email" class="form-control" id="exampleInputEmail1" v-model="email" placeholder="Escriba su correo electrónico" required>
              </div>
              <div class="d-grid gap-2">
                <button type="submit" class="btn btn-primary d-block">Enviar</button>
              </div>
              <span class="d-block text-center mt-3" style="color: #6c757d; cursor: pointer;" @click="irALogin">Inicia sesión si ya tiene cuenta </span>
            </form>
          </div>
        </div>
        <div v-if="mensaje" class="alert alert-success mt-3" role="alert">
          {{ mensaje }}
        </div>
        <div v-if="error" class="alert alert-danger mt-3" role="alert">
          {{ error }}
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
.container {
  padding-top: 50px;
}

.recovery-form {
  padding: 20px;
  border: 1px solid #ccc;
  border-radius: 8px;
  background-color: #f9f9f9;
}

.form-group {
  margin-bottom: 20px;
}

.btn-primary {
  background-color: #007bff;
  border-color: #007bff;
}

.btn-primary:hover {
  background-color: #0056b3;
  border-color: #0056b3;
}

</style>
