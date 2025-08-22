<template>
  <div class="incidence-form">
    <h1>Generar Incidencia</h1>
    <button class="secondary-btn" @click="goToIncidences">Ver Incidencias Guardadas</button>
    <div class="selectors">
      <label>Lugar:</label>
      <select v-model="placeSelected" class="input">
        <option disabled value="">Selecciona lugar</option>
        <option v-for="(items, place) in data" :key="place" :value="place">{{ place }}</option>
      </select>
      <div v-if="availableItems.length">
        <label v-if="placeSelected">Item:</label>
        <select v-if="placeSelected" v-model="itemSelected" class="input">
          <option disabled value="">Selecciona item</option>
          <option v-for="(details, item) in data[placeSelected]" :key="item" :value="item">
            {{ item }}
          </option>
        </select>
      </div>
      <div v-if="availableDetails.length">
        <label v-if="itemSelected">Detalle:</label>
        <select v-if="itemSelected" v-model="detailSelected" class="input">
          <option disabled value="">Selecciona detalle</option>
          <option
            v-for="(problems, detail) in data[placeSelected][itemSelected]"
            :key="detail"
            :value="detail"
          >
            {{ detail }}
          </option>
        </select>
      </div>
      <div v-if="availableProblems.length">
        <label v-if="detailSelected">Problema:</label>
        <select v-if="detailSelected" v-model="problemSelected" class="input">
          <option disabled value="">Selecciona problema</option>
          <option
            v-for="problem in data[placeSelected][itemSelected][detailSelected]"
            :key="problem"
            :value="problem"
          >
            {{ problem }}
          </option>
        </select>
      </div>
      <label>Observación:</label>
      <textarea
        v-model="observation"
        rows="4"
        cols="50"
        placeholder="Escribe una observación..."
        class="input"
      ></textarea>
      <button class="primary-btn" @click="send">Enviar</button>
    </div>
  </div>
</template>

<script setup>
import { ref, computed, watch } from 'vue'
import { incidenceData as data } from '@/data/incidences'
import Swal from 'sweetalert2'

const placeSelected = ref('')
const itemSelected = ref('')
const detailSelected = ref('')
const problemSelected = ref('')
const observation = ref('')

watch(placeSelected, () => {
  itemSelected.value = ''
  detailSelected.value = ''
  problemSelected.value = ''
})

watch(itemSelected, () => {
  detailSelected.value = ''
  problemSelected.value = ''
})

watch(detailSelected, () => {
  problemSelected.value = ''
})

const availableItems = computed(() => {
  return placeSelected.value ? Object.keys(data[placeSelected.value]) : []
})
const availableDetails = computed(() => {
  return placeSelected.value && itemSelected.value
    ? Object.keys(data[placeSelected.value][itemSelected.value])
    : []
})
const availableProblems = computed(() => {
  return placeSelected.value && itemSelected.value && detailSelected.value
    ? data[placeSelected.value][itemSelected.value][detailSelected.value]
    : []
})

const resetForm = () => {
  placeSelected.value = ''
  itemSelected.value = ''
  detailSelected.value = ''
  problemSelected.value = ''
  observation.value = ''
}

const send = () => {
  if (
    !placeSelected.value ||
    !itemSelected.value ||
    !detailSelected.value ||
    !problemSelected.value
  ) {
    Swal.fire({
      title: 'Error',
      text: 'Por favor complete todos los campos obligatorios',
      icon: 'error',
    })
    return
  }

  console.log({
    lugar: placeSelected.value,
    item: itemSelected.value,
    detalle: detailSelected.value,
    problema: problemSelected.value,
    observacion: observation.value,
  })

  Swal.fire({
    title: '¡Éxito!',
    text: 'La incidencia se ha generado correctamente',
    icon: 'success',
    confirmButtonText: 'Aceptar',
  }).then(() => {
    resetForm()
  })
}
</script>

<style scoped>
.incidence-form {
  max-width: 500px;
  margin: 2rem auto;
  padding: 2rem 2.5rem;
  background: #fff;
  border-radius: 16px;
  box-shadow: 0 4px 24px rgba(0, 0, 0, 0.08), 0 1.5px 6px rgba(0, 0, 0, 0.04);
  font-family: 'Segoe UI', 'Roboto', Arial, sans-serif;
}
h1 {
  text-align: center;
  font-size: 2rem;
  font-weight: 600;
  margin-bottom: 1.5rem;
  color: #1a237e;
  letter-spacing: 0.5px;
}
.selectors {
  margin-top: 1rem;
}
label {
  display: block;
  margin-top: 18px;
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
  box-shadow: 0 2px 8px rgba(26, 35, 126, 0.08);
  cursor: pointer;
  transition: background 0.2s;
}
.primary-btn:hover {
  background: linear-gradient(90deg, #3949ab 60%, #1a237e 100%);
}
.secondary-btn {
  display: block;
  margin: 0 auto 1.5rem auto;
  padding: 0.7rem 1.5rem;
  background: #e3e7ee;
  color: #1a237e;
  font-weight: 500;
  border: none;
  border-radius: 8px;
  font-size: 1rem;
  cursor: pointer;
  transition: background 0.2s;
}
.secondary-btn:hover {
  background: #cfd8dc;
}
textarea.input {
  resize: vertical;
  min-height: 80px;
  max-height: 200px;
}
</style>
