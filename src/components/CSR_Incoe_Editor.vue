<template>
  <v-dialog v-model="dialog" persistent max-width="600">
    <v-card>
      <v-card-title>
        Edit Konten CSR
      </v-card-title>
      <v-card-text>
        <v-text-field v-model="form.subtitle" label="Subtitle"></v-text-field>

        <v-text-field v-model="form.caption" label="Caption"></v-text-field>

        <v-textarea v-model="form.text1" label="Text Paragraf 1"></v-textarea>

        <v-textarea v-model="form.text2" label="Text Paragraf 2"></v-textarea>

        <v-file-input label="Gambar Utama" @change="onFileChange($event, 'mainImage')"></v-file-input>

        <v-file-input label="Gambar Kecil Atas" @change="onFileChange($event, 'topImage')"></v-file-input>

        <v-file-input label="Gambar Bawah 1" @change="onFileChange($event, 'bottomImage1')"></v-file-input>

        <v-file-input label="Gambar Bawah 2" @change="onFileChange($event, 'bottomImage2')"></v-file-input>
      </v-card-text>

      <v-card-actions>
        <v-spacer></v-spacer>
        <v-btn color="primary" @click="applyChanges">Terapkan</v-btn>
        <v-btn text @click="dialog = false">Tutup</v-btn>
      </v-card-actions>
    </v-card>
  </v-dialog>

  <div class="preview-section">
    <CSRPreview :content="form" />
  </div>
</template>

<script setup>
import { reactive, ref } from 'vue';
import CSRPreview from './Template.vue';

const dialog = ref(true);

const form = reactive({
  subtitle: 'INCOE BERBAGI ILMU',
  caption: 'Doc. Kunjungan CSR SDN Parungmulya Kelas Jauh.',
  text1: 'Sebagai wujud komitmen terhadap pembangunan berkelanjutan...',
  text2: 'Program Incoe Berbagi Ilmu ini mulai dijalankan pada tanggal 30 April 2025...',
  mainImage: '',
  topImage: '',
  bottomImage1: '',
  bottomImage2: '',
});

const onFileChange = (event, key) => {
  const file = event.target.files[0];
  if (file) {
    const reader = new FileReader();
    reader.onload = (e) => {
      form[key] = e.target.result;
    };
    reader.readAsDataURL(file);
  }
};

const applyChanges = () => {
  dialog.value = false;
};
</script>

<style scoped>
.preview-section {
  border: 1px solid #ccc;
  padding: 10px;
  overflow: auto;
}
</style>