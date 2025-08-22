<template>
  <div class="login-card">
    <h2>Iniciar Sesión</h2>
    <form @submit.prevent="handleLogin">
      <label for="email">Email:</label>
      <input v-model="email" id="email" type="email" placeholder="Email" required class="input" />

      <label for="password">Contraseña:</label>
      <input v-model="password" id="password" type="password" placeholder="Contraseña" required class="input" />

      <button type="submit" class="primary-btn">Iniciar Sesión</button>
    </form>
  </div>
</template>
<script setup>
import { ref } from 'vue'
import { useRouter } from 'vue-router'
import { useUserStore } from '@/stores/user'

const email = ref('')
const password = ref('')
const router = useRouter()
const userStore = useUserStore()

const handleLogin = async () => {
  try {
    await userStore.login({ email: email.value, password: password.value })
    await userStore.fetchUser()
    router.push('/incidences')
  } catch (error) {
    console.error('Error al iniciar sesión:', error)
    alert('Login Fallido. Revisa tus credenciales.')
  }
}
</script>
<style scoped>
.login-card {
  max-width: 400px;
  margin: 3rem auto;
  padding: 2rem 2.5rem;
  background: #fff;
  border-radius: 16px;
  box-shadow: 0 4px 24px rgba(0,0,0,0.08), 0 1.5px 6px rgba(0,0,0,0.04);
  font-family: 'Segoe UI', 'Roboto', Arial, sans-serif;
}

h2 {
  text-align: center;
  font-size: 1.7rem;
  font-weight: 600;
  margin-bottom: 1.5rem;
  color: #1a237e;
  letter-spacing: 0.5px;
}

form {
  display: flex;
  flex-direction: column;
}

label {
  margin-top: 14px;
  margin-bottom: 6px;
  font-weight: 500;
  color: #374151;
  font-size: 1rem;
}

.input {
  width: 100%;
  padding: 0.7rem 1rem;
  border: 1px solid #cfd8dc;
  border-radius: 8px;
  background: #f7fafc;
  font-size: 1rem;
  margin-bottom: 8px;
  transition: border-color 0.2s;
}

.input:focus {
  border-color: #1a237e;
  outline: none;
  background: #fff;
}

.primary-btn {
  width: 100%;
  margin-top: 1.5rem;
  padding: 0.9rem 0;
  background: linear-gradient(90deg, #1a237e 60%, #3949ab 100%);
  color: #fff;
  font-weight: 600;
  font-size: 1.1rem;
  border: none;
  border-radius: 8px;
  box-shadow: 0 2px 8px rgba(26,35,126,0.08);
  cursor: pointer;
  transition: background 0.2s;
}

.primary-btn:hover {
  background: linear-gradient(90deg, #3949ab 60%, #1a237e 100%);
}
</style>
