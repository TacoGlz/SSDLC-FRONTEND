<template>
  <div class="incidence-list">
    <div class="header">
      <img src="https://cdn-icons-png.flaticon.com/512/1828/1828640.png" alt="Incidencias" class="icon" />
      <h2>Mis Incidencias</h2>
    </div>
    <button class="logout-btn" @click="logout">Cerrar Sesión</button>
    <router-link to="/generate_incidence">
      <button class="primary-btn">Generar Nueva Incidencia</button>
    </router-link>
    <div v-if="incidences.length">
      <div v-for="i in incidences" :key="i.id" class="incidence-item">
        <h3>
          {{ i.type }}
          <span class="status" :class="i.status">{{ i.status }}</span>
        </h3>
        <p><strong>Ubicación:</strong> {{ i.location }}</p>
        <p>{{ i.description }}</p>
        <p v-if="i.attachment">
          <a :href="getFileUrl(i.attachment)" target="_blank">Ver adjunto</a>
        </p>
        <small>Registrado el: {{ formateDate(i.created_at) }}</small>
      </div>
    </div>
    <p v-else>No hay incidencias registradas.</p>
  </div>
</template>
<script setup>
import { ref, onMounted } from 'vue'
import axios from '@/api/axios'
import { useUserStore } from '@/stores/user'
import { useRouter } from 'vue-router'

const incidences = ref([])
const userStore = useUserStore()
const router = useRouter()

onMounted(async () => {
  try {
    const res = await axios.get('/api/reports')
    incidences.value = res.data
  } catch (error) {
    console.error('Error al obtener reportes:', error)
    router.push('/login')
  }
})

const logout = async () => {
  await userStore.logout()
  router.push('/login')
}

const getFileUrl = (path) => {
  return 'http://127.0.0.1:8000/storage/${path}'
}

const formateDate = (datetime) => {
  return new Date(datetime).toLocaleDateString()
}
</script>
<style scoped>
.incidence-list {
  max-width: 700px;
  margin: 2rem auto;
  padding: 2rem 2.5rem;
  background: #f9fafb;
  border-radius: 18px;
  box-shadow: 0 6px 32px rgba(0,0,0,0.10), 0 2px 8px rgba(0,0,0,0.04);
  font-family: 'Segoe UI', 'Roboto', Arial, sans-serif;
  border: 1px solid #e3e7ee;
}

.header {
  display: flex;
  align-items: center;
  gap: 1rem;
  margin-bottom: 1.5rem;
  justify-content: center;
}

.icon {
  width: 38px;
  height: 38px;
}

h2 {
  text-align: center;
  font-size: 1.7rem;
  font-weight: 600;
  color: #1a237e;
  letter-spacing: 0.5px;
  margin: 0;
}

.logout-btn {
  display: block;
  margin: 0 auto 1.5rem auto;
  padding: 0.7rem 1.5rem;
  background: #e11d48;
  color: #fff;
  font-weight: 500;
  border: none;
  border-radius: 8px;
  font-size: 1rem;
  cursor: pointer;
  transition: background 0.2s;
}

.logout-btn:hover {
  background: #be123c;
}

.primary-btn {
  display: block;
  width: 100%;
  margin: 0 auto 2rem auto;
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

.incidence-item {
  background: #fff;
  padding: 1.2rem 1rem;
  margin-bottom: 1.2rem;
  border-left: 5px solid #1d4ed8;
  border-radius: 8px;
  box-shadow: 0 2px 8px rgba(0,0,0,0.04);
}

.status {
  font-size: 0.9rem;
  margin-left: 1rem;
  padding: 0.2rem 0.7rem;
  border-radius: 4px;
  text-transform: uppercase;
  font-weight: 500;
  letter-spacing: 0.5px;
}

.status.pending {
  background: #fde68a;
  color: #92400e;
}
.status.in_review {
  background: #bfdbfe;
  color: #1e40af;
}
.status.resolved {
  background: #bbf7d0;
  color: #166534;
}
</style>
