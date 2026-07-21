<template>
  <div class="card shadow-sm mb-4 border-0 bg-success-gradient">
    <div class="card-body p-4">
      <h2 class="text-center mb-4 text-white">
        <i :class="props.siswaEdit ? 'bi bi-pencil-square me-2' : 'bi bi-plus-circle me-2'"></i>
        {{ props.siswaEdit ? 'Edit Siswa' : 'Tambah Siswa' }}
      </h2>

      <form @submit.prevent="tambahAtauEditSiswa" class="d-flex align-items-center gap-3">
        <!-- Input Fields -->
        <div class="flex-grow-1">
          <div class="row g-2 align-items-center">
            <div class="col-md-4">
              <div class="input-group input-group-sm">
                <span class="input-group-text bg-white"><i class="bi bi-person-fill text-success"></i></span>
                <input v-model="nama" type="text" class="form-control" placeholder="Nama Siswa" required />
              </div>
            </div>
            <div class="col-md-4">
              <div class="input-group input-group-sm">
                <span class="input-group-text bg-white"><i class="bi bi-geo-alt-fill text-success"></i></span>
                <input v-model="alamat" type="text" class="form-control" placeholder="Alamat" required />
              </div>
            </div>
            <div class="col-md-2">
              <div class="input-group input-group-sm">
                <span class="input-group-text bg-white"><i class="bi bi-calendar-event-fill text-success"></i></span>
                <input v-model="umur" type="number" class="form-control" placeholder="Umur" required />
              </div>
            </div>
            <div class="col-md-2">
              <div class="d-flex gap-1">
                <button type="submit" class="btn btn-light text-success fw-bold btn-action flex-grow-1">
                  <i :class="props.siswaEdit ? 'bi bi-check-lg me-1' : 'bi bi-plus-lg me-1'"></i>
                  {{ props.siswaEdit ? 'Update' : 'Tambah' }}
                </button>
                <button
                  v-if="props.siswaEdit"
                  @click="resetForm"
                  type="button"
                  class="btn btn-outline-light fw-bold btn-action"
                >
                  <i class="bi bi-x-lg"></i>
                </button>
              </div>
            </div>
          </div>
        </div>

        <!-- Avatar Foto Upload — pojok kanan -->
        <div class="avatar-upload-wrapper">
          <input
            ref="fileInput"
            type="file"
            accept="image/jpeg,image/png,image/gif,image/webp"
            class="d-none"
            @change="onFileSelected"
          />
          <div
            class="avatar-upload"
            @click="triggerFileInput"
            :title="fotoPreview ? 'Klik untuk ganti foto' : 'Klik untuk upload foto'"
          >
            <img v-if="fotoPreview" :src="fotoPreview" class="avatar-img" alt="Preview" />
            <div v-else class="avatar-empty">
              <i class="bi bi-person-fill"></i>
            </div>
            <!-- Hover overlay -->
            <div class="avatar-overlay">
              <i class="bi bi-camera-fill"></i>
              <span>Foto</span>
            </div>
            <!-- Loading overlay -->
            <div v-if="isUploading" class="avatar-loading">
              <div class="spinner-border spinner-border-sm text-white"></div>
            </div>
          </div>
          <!-- Tombol hapus — badge X kecil di pojok kanan atas -->
          <button
            v-if="fotoPreview && !isUploading"
            type="button"
            class="avatar-clear"
            @click.stop="clearFoto"
            title="Hapus foto"
          >
            <i class="bi bi-x"></i>
          </button>
        </div>
      </form>
    </div>
  </div>
</template>

<script setup>
import { ref, watch } from 'vue'
import axios from 'axios'
import Swal from 'sweetalert2'
import API_BASE_URL from '../env.js'

const props = defineProps({
  siswaEdit: Object,
})

const emit = defineEmits(['siswa-added', 'cancel-edit'])

const nama = ref('')
const alamat = ref('')
const umur = ref(0)
const fotoFilename = ref(null)  // nama file yang sudah diupload ke server
const fotoPreview = ref(null)   // URL preview di browser
const fileInput = ref(null)
const isUploading = ref(false)

const API_URL = `${API_BASE_URL}/siswa`
const UPLOAD_URL = `${API_BASE_URL}/upload`

watch(
  () => props.siswaEdit,
  (newSiswa) => {
    if (newSiswa) {
      nama.value = newSiswa.nama
      alamat.value = newSiswa.alamat
      umur.value = newSiswa.umur
      fotoFilename.value = newSiswa.foto || null
      fotoPreview.value = newSiswa.foto
        ? `${API_BASE_URL}/uploads/${newSiswa.foto}`
        : null
    }
  },
  { immediate: true }
)

const triggerFileInput = () => {
  fileInput.value?.click()
}

const onFileSelected = async (event) => {
  const file = event.target.files[0]
  if (!file) return

  // Preview lokal dulu (responsif)
  fotoPreview.value = URL.createObjectURL(file)

  isUploading.value = true
  try {
    const formData = new FormData()
    formData.append('file', file)
    const res = await axios.post(UPLOAD_URL, formData, {
      headers: { 'Content-Type': 'multipart/form-data' },
    })
    fotoFilename.value = res.data.filename
  } catch (err) {
    Swal.fire({
      icon: 'error',
      title: 'Upload Gagal',
      text: err.response?.data?.error || 'Gagal mengupload gambar',
      confirmButtonColor: '#198754',
    })
    clearFoto()
  } finally {
    isUploading.value = false
    if (fileInput.value) fileInput.value.value = ''
  }
}

const clearFoto = () => {
  fotoPreview.value = null
  fotoFilename.value = null
  if (fileInput.value) fileInput.value.value = ''
}

const resetForm = () => {
  nama.value = ''
  alamat.value = ''
  umur.value = 0
  clearFoto()
  emit('cancel-edit')
}

const showSuccessAlert = (message) => {
  Swal.fire({
    icon: 'success',
    title: 'Sukses!',
    text: message,
    timer: 2000,
    showConfirmButton: false,
    background: '#f8f9fa',
    position: 'center',
    width: 400,
  })
}

const showErrorAlert = (message) => {
  Swal.fire({
    icon: 'error',
    title: 'Gagal!',
    text: message,
    background: '#f8f9fa',
    position: 'center',
    confirmButtonColor: '#198754',
  })
}

const tambahAtauEditSiswa = async () => {
  try {
    const payload = {
      nama: nama.value,
      alamat: alamat.value,
      umur: umur.value,
      foto: fotoFilename.value || null,
    }

    if (props.siswaEdit) {
      await axios.put(`${API_URL}/${props.siswaEdit.id}`, payload)
      showSuccessAlert('Data siswa berhasil diperbarui')
    } else {
      await axios.post(API_URL, payload)
      showSuccessAlert('Data siswa berhasil ditambahkan')
    }

    resetForm()
    emit('siswa-added')
  } catch (err) {
    showErrorAlert(err.response?.data?.message || 'Gagal menyimpan data siswa')
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

.btn-action {
  height: 31px;
  padding: 0.25rem 0.75rem;
  font-size: 0.8125rem;
  display: inline-flex;
  align-items: center;
  justify-content: center;
  white-space: nowrap;
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

/* === Avatar Upload (pojok kanan) === */
.avatar-upload-wrapper {
  position: relative;
  flex-shrink: 0;
}

.avatar-upload {
  width: 72px;
  height: 72px;
  border-radius: 50%;
  cursor: pointer;
  position: relative;
  overflow: hidden;
  border: 3px solid rgba(255, 255, 255, 0.7);
  box-shadow: 0 0 0 2px rgba(255, 255, 255, 0.2), 0 4px 12px rgba(0, 0, 0, 0.25);
  transition: border-color 0.2s, box-shadow 0.2s;
  background: rgba(255, 255, 255, 0.12);
}

.avatar-upload:hover {
  border-color: #fff;
  box-shadow: 0 0 0 3px rgba(255, 255, 255, 0.4), 0 6px 16px rgba(0, 0, 0, 0.3);
}

.avatar-upload:hover .avatar-overlay {
  opacity: 1;
}

.avatar-img {
  width: 100%;
  height: 100%;
  object-fit: cover;
  display: block;
}

.avatar-empty {
  width: 100%;
  height: 100%;
  display: flex;
  align-items: center;
  justify-content: center;
  color: rgba(255, 255, 255, 0.8);
  font-size: 2.2rem;
}

.avatar-overlay {
  position: absolute;
  inset: 0;
  background: rgba(0, 0, 0, 0.5);
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  gap: 2px;
  color: #fff;
  font-size: 0.6rem;
  opacity: 0;
  transition: opacity 0.2s ease;
}

.avatar-overlay i {
  font-size: 1.3rem;
}

.avatar-loading {
  position: absolute;
  inset: 0;
  background: rgba(0, 0, 0, 0.55);
  display: flex;
  align-items: center;
  justify-content: center;
}

/* Badge X hapus foto di pojok kanan atas avatar */
.avatar-clear {
  position: absolute;
  top: -3px;
  right: -3px;
  width: 20px;
  height: 20px;
  border-radius: 50%;
  background: #dc3545;
  border: 2px solid #fff;
  color: #fff;
  font-size: 0.65rem;
  display: flex;
  align-items: center;
  justify-content: center;
  cursor: pointer;
  padding: 0;
  line-height: 1;
  box-shadow: 0 1px 4px rgba(0, 0, 0, 0.3);
  transition: background 0.15s;
}

.avatar-clear:hover {
  background: #b02a37;
}

/* Dark mode */
.dark-mode .card-body *:not(.btn):not(i),
.dark-mode .card-body,
.dark-mode .card-body h2,
.dark-mode .card-body input,
.dark-mode .card-body .input-group-text {
  color: #4caf50 !important;
}

.dark-mode .card-body input::placeholder {
  color: #4caf50 !important;
  opacity: 0.7 !important;
}

.dark-mode .bg-success-gradient {
  background: linear-gradient(135deg, #1a3e1a 0%, #1a4a42 100%) !important;
}

.dark-mode .input-group-text {
  background-color: var(--dark-input) !important;
  border-color: var(--dark-border) !important;
}

.dark-mode .form-control {
  background-color: var(--dark-input) !important;
  border-color: var(--dark-border) !important;
  color: #4caf50 !important;
}

.dark-mode .btn-light {
  background-color: var(--dark-input) !important;
  border-color: var(--dark-border) !important;
}

.dark-mode .btn-outline-light {
  border-color: var(--dark-border) !important;
  color: #4caf50 !important;
}

.dark-mode .btn-outline-light:hover {
  background-color: rgba(25, 135, 84, 0.2) !important;
}
</style>
