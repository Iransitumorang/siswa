<template>
  <div class="bg-white rounded-2xl shadow-xl p-6 w-full max-w-xl mx-auto">
    <h2 class="text-2xl font-bold text-indigo-700 mb-4">
      {{ props.siswaEdit ? '✏️ Edit Siswa' : '➕ Tambah Siswa' }}
    </h2>
    <form @submit.prevent="tambahAtauEditSiswa" class="space-y-4">
      <input
        v-model="nama"
        placeholder="Nama"
        required
        class="w-full px-4 py-3 rounded-xl border border-indigo-200 focus:ring-2 focus:ring-indigo-500 outline-none"
      />
      <input
        v-model="alamat"
        placeholder="Alamat"
        required
        class="w-full px-4 py-3 rounded-xl border border-indigo-200 focus:ring-2 focus:ring-indigo-500 outline-none"
      />
      <input
        v-model="umur"
        type="number"
        placeholder="Umur"
        required
        class="w-full px-4 py-3 rounded-xl border border-indigo-200 focus:ring-2 focus:ring-indigo-500 outline-none"
      />
      <button
        type="submit"
        class="w-full py-3 rounded-xl bg-indigo-600 hover:bg-indigo-700 text-white font-semibold tracking-wide shadow-lg transition"
      >
        {{ props.siswaEdit ? 'Update Siswa' : 'Tambah Siswa' }}
      </button>
    </form>
  </div>
</template>

<script setup>
import { ref, watch } from 'vue'
import axios from 'axios'

const props = defineProps({
  siswaEdit: Object,
})

const emit = defineEmits(['siswa-added'])

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

    nama.value = ''
    alamat.value = ''
    umur.value = 0
    emit('siswa-added')
  } catch (err) {
    console.error('Gagal simpan:', err)
  }
}
</script>
