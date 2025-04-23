<template>
    <div>
      <h2>Daftar Siswa</h2>
      <table>
        <thead>
          <tr>
            <th>Nama</th>
            <th>NIS</th>
            <th>Aksi</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="siswa in siswaList" :key="siswa.id">
            <td>{{ siswa.nama }}</td>
            <td>{{ siswa.nis }}</td>
            <td>
              <button @click="$emit('edit', siswa)">Edit</button>
              <button @click="hapusSiswa(siswa.id)">Hapus</button>
            </td>
          </tr>
        </tbody>
      </table>
    </div>
  </template>
  
  <script setup>
  import { ref, onMounted } from 'vue'
  const siswaList = ref([])
  
  const fetchSiswa = async () => {
    const res = await fetch('http://localhost:8081/siswa')
    siswaList.value = await res.json()
  }
  
  const hapusSiswa = async (id) => {
    await fetch(`http://localhost:8081/siswa/${id}`, { method: 'DELETE' })
    fetchSiswa()
  }
  
  onMounted(fetchSiswa)
  </script>
  