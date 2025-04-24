<template>
  <div class="card shadow-sm mb-4 border-0 bg-success-gradient">
    <div class="card-body p-4">
      <h2 class="text-center mb-4 text-white">
        <i :class="props.siswaEdit ? 'bi bi-pencil-square me-2' : 'bi bi-plus-circle me-2'"></i>
        {{ props.siswaEdit ? 'Edit Siswa' : 'Tambah Siswa' }}
      </h2>
      
      <form @submit.prevent="tambahAtauEditSiswa" class="row g-3 align-items-center">
        
        <div class="col-md-3 ms-5">
          <div class="input-group input-group-sm">
            <span class="input-group-text bg-white"><i class="bi bi-person-fill text-success"></i></span>
            <input
              v-model="nama"
              type="text"
              class="form-control"
              placeholder="Nama Siswa"
              required
            />
          </div>
        </div>
        
        <div class="col-md-3">
          <div class="input-group input-group-sm">
            <span class="input-group-text bg-white"><i class="bi bi-geo-alt-fill text-success"></i></span>
            <input
              v-model="alamat"
              type="text"
              class="form-control"
              placeholder="Alamat"
              required
            />
          </div>
        </div>
        
        <div class="col-md-2">
          <div class="input-group input-group-sm">
            <span class="input-group-text bg-white"><i class="bi bi-calendar-event-fill text-success"></i></span>
            <input
              v-model="umur"
              type="number"
              class="form-control"
              placeholder="Umur"
              required
            />
          </div>
        </div>
        
        <div class="col-md-2">
          <div class="d-flex h-100">
            <button
              type="submit"
              class="btn btn-light text-success fw-bold btn-action"
            >
              <i :class="props.siswaEdit ? 'bi bi-check-lg me-1' : 'bi bi-plus-lg me-1'"></i>
              {{ props.siswaEdit ? 'Update' : 'Tambah' }}
            </button>
            <button
              v-if="props.siswaEdit"
              @click="resetForm"
              type="button"
              class="btn btn-outline-light text-white fw-bold btn-action ms-2"
            >
              <i class="bi bi-x-lg me-1"></i>
              Cancel
            </button>
          </div>
        </div>
      </form>
    </div>
  </div>
</template>

<script setup>
import { ref, watch } from 'vue'
import axios from 'axios'

const props = defineProps({
  siswaEdit: Object,
})

const emit = defineEmits(['siswa-added', 'cancel-edit'])

const nama = ref('')
const alamat = ref('')
const umur = ref(0)

watch(
  () => props.siswaEdit,
  (newSiswa) => {
    if (newSiswa) {
      nama.value = newSiswa.nama
      alamat.value = newSiswa.alamat
      umur.value = newSiswa.umur
    }
  },
  { immediate: true }
)

const resetForm = () => {
  nama.value = ''
  alamat.value = ''
  umur.value = 0
  emit('cancel-edit')
}

const API_URL = 'http://localhost:8081/siswa'

const tambahAtauEditSiswa = async () => {
  try {
    if (props.siswaEdit) {
      await axios.put(`${API_URL}/${props.siswaEdit.id}`, {
        nama: nama.value,
        alamat: alamat.value,
        umur: umur.value,
      })
    } else {
      await axios.post(API_URL, {
        nama: nama.value,
        alamat: alamat.value,
        umur: umur.value,
      })
    }

    resetForm()
    emit('siswa-added')
  } catch (err) {
    console.error('Gagal simpan:', err)
  }
}
</script>

<style scoped>
.bg-success-gradient {
  background: linear-gradient(135deg, #198754 0%, #2a9d8f 100%);
  border-radius: 12px;
}

.input-group-text {
  border-right: none;
}

.form-control {
  border-left: none;
}

.btn-outline-light:hover {
  background-color: rgba(255, 255, 255, 0.2);
}

/* Perfect button sizing */
.btn-action {
  height: 31px;
  padding: 0.25rem 0.75rem;
  font-size: 0.8125rem;
  display: inline-flex;
  align-items: center;
  justify-content: center;
}

.input-group-sm {
  height: 31px;
}

.input-group-sm .form-control,
.input-group-sm .input-group-text {
  height: 100%;
  padding: 0.25rem 0.5rem;
  font-size: 0.8125rem;
}
</style>