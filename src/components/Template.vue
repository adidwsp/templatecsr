<template>
  <div>
    <div ref="a4Page" class="a4-page">
      <div class="blue-frame"></div>

      <div class="content">
          <div class="title-column">
              <div class="title-row">
                  <h1 class="title">{{ form.title }}</h1>
                  <h2 class="subtitle">{{ form.subtitle }}</h2>
              </div>
              <img :src="form.smallImage" alt="image" class="image-small" />
          </div>
          <img :src="form.largeImage" alt="image" class="image-large" />

        <p class="caption">{{ form.caption }}</p>
          <p class="first-paragraph">{{ form.firstParagraph }}</p>
        
        <div class="text-section">
          <div class="image-col">
              <img :src="form.colImage1" class="image-col1" alt="image" />
              <img :src="form.colImage2" class="image-col2" alt="image" />
          </div>
          <p class="column">
            {{ form.columnText }}
          </p>
        </div>
        
      </div>
      <div class="footer">{{ form.footer }}</div>
    </div>
    <div class="export-btns">
      <v-btn prepend-icon="mdi-file-pdf-box" @click="exportToPNG">
          Export PNG
      </v-btn>
      <v-btn prepend-icon="mdi-file-png-box" @click="exportToPDF">
          Export PDF
      </v-btn>
      
      <v-btn prepend-icon="mdi-home" to="/">
          Home
      </v-btn>
      <v-btn prepend-icon="mdi-pencil" @click="dialog = true"> 
          Edit
      </v-btn>
    </div>

    <!-- Edit Dialog -->
    <v-dialog v-model="dialog" max-width="600px">
      <v-card>
        <v-card-title>Edit Content</v-card-title>
        <v-card-text>
          <v-form>
            <v-text-field v-model="form.title" label="Title" />
            <v-text-field v-model="form.subtitle" label="Subtitle" />
            <!-- <v-text-field v-model="form.smallImage" label="Small Image URL" /> -->
            <!-- <v-text-field v-model="form.largeImage" label="Large Image URL" /> -->
            <v-file-input
              v-model="form.largeImage"
              label="Upload Large Image"
              accept="image/*"
              show-size
              prepend-icon="mdi-image"
            />
            <v-text-field v-model="form.caption" label="Caption" />
            <v-textarea v-model="form.firstParagraph" label="First Paragraph" rows="3" />
            <!-- <v-text-field v-model="form.colImage1" label="Column Image 1 URL" />
            <v-text-field v-model="form.colImage2" label="Column Image 2 URL" /> -->
            <v-file-input
              v-model="form.colImage1"
              label="Upload Column Image 1"
              accept="image/*"
              show-size
              prepend-icon="mdi-image"
            />
            
            <v-file-input
              v-model="form.colImage2"
              label="Upload Column Image 2"
              accept="image/*"
              show-size
              prepend-icon="mdi-image"
            />
            <v-textarea v-model="form.columnText" label="Column Text" rows="4" />
            <v-text-field v-model="form.footer" label="Footer Text" />
          </v-form>
        </v-card-text>
        <v-card-actions>
          <v-spacer />
          <v-btn text @click="dialog = false">Cancel</v-btn>
          <v-btn color="primary" text @click="dialog = false">Save</v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
  </div>
</template>

<script setup>
import html2canvas from "html2canvas";
import jsPDF from "jspdf";
import { ref, reactive } from "vue";

const a4Page = ref(null);
const dialog = ref(false);

const form = reactive({
  title: "CERITA",
  subtitle: "INCOE BERBAGI ILMU",
  smallImage: "https://lh3.googleusercontent.com/pw/AP1GczPF0nbwZmBF-Yyqt7_71j4D-GHq2mheTokgcvhlMOITY3cJhvAPRs-YIZYQWZHNvvlW5SwE2jgCuX_ZidZOHe3_iSPOqDdJ1wcLjOkXamnPhkcPU3p6Wl_e780kl6HsDb1Yj37hp72XGegfjVJciJC10w=w500-h500-s-no-gm?authuser=0",
  largeImage: "https://placehold.co/179x53",
  caption: "Doc. Kunjungan CSR SDN Parungmulya Kelas Jauh.",
  firstParagraph: `Sebagai wujud komitmen terhadap pembangunan berkelanjutan, PT Century Batteries Indonesia secara konsisten menjalankan program Corporate Social Responsibility (CSR) di bidang pendidikan, yang bertujuan untuk meningkatkan kualitas sumber daya manusia sejak usia dini hingga jenjang pendidikan tinggi.`,  
  colImage1: "https://placehold.co/241x173",
  colImage2: "https://placehold.co/241x173",
  columnText: `Program Incoe Berbagi Ilmu ini mulai dijalankan pada tanggal 30 April 2025 di SD binaan PT Century Batteries Indonesia, yaitu SDN Parungmulya Kelas Jauh.\n\nPada kesempatan kali ini PIC CSR PT Century Batteries Indonesia memberikan edukasi mengenai Keselamatan dan Keamanan di sekolah. Kegiatan ini diikuti oleh 56 orang penerima manfaat, yang terdiri dari 40 orang siswa, dan 16 orang guru dengan harapan dapat meningkatkan kesadaran serta kesiapsiagaan terhadap potensi risiko di sekolah.`,
  footer: "#02 CSR INCOE DALAM BERITA"
});

const exportToPNG = async () => {
  const canvas = await html2canvas(a4Page.value);
  const link = document.createElement("a");
  link.download = "incoe-csr.png";
  link.href = canvas.toDataURL();
  link.click();
};
const exportToPDF = async () => {
  const canvas = await html2canvas(a4Page.value);
  const imgData = canvas.toDataURL("image/png");
  const pdf = new jsPDF("p", "mm", "a4");
  pdf.addImage(imgData, "PNG", 0, 0, 210, 297);
  pdf.save("incoe-csr.pdf");
};
</script>

<style scoped>
.a4-page {
  width: 210mm;
  height: 297mm;
  margin: auto;
  background: white;
  position: relative;
  box-shadow: 0 0 5px rgba(0,0,0,0.1);
  padding: 16mm;
  box-sizing: border-box;
}

.title-column {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 20px;
}

.blue-frame::before,
.blue-frame::after {
  content: "";
  position: absolute;
  width: 30px;
  background: #013184;
}

.blue-frame::before {
  left: 0;
  height: 80%;
  border-radius: 0px 50px 50px 0px;
}

.blue-frame::after {
  right: 0;
  height: 80%;
  border-radius: 50px 0px 0px 50px;
}

.title {
  font-size: 86.2px;
  font-weight: bold;
  color: black;
  font-family: "Playfair Display", serif;
  font-optical-sizing: auto;
  font-style: normal;
}

.subtitle {
  font-family: "Montserrat", sans-serif;
  font-size: 40px;
  margin-bottom: 20px;
  color: black;
  font-weight: normal;
}

.image-large {
  width: 179mm;
  height: 53mm;
  object-fit: cover;
  border-radius: 10px;
}

.image-small {
  width: 46mm;
  height: 46mm;
  object-fit: cover;
}

.caption {
  font-size: 10px;
  margin: 4px 0 16px;
  color: black;
}

.first-paragraph,
.column {
  font-size: 18px;
  text-align: justify;
  color: black;
  white-space: pre-wrap;
}

.text-section {
  display: flex;
  gap: 10px;
  margin-top: 10px;
}

.image-col {
  display: flex;
  flex-direction: column;
  gap: 10px;
  margin-top: 5px;
}

.image-col1 {
  width: 64mm;
  height: 44mm;
  object-fit: cover;
  border-radius: 10px;
}
.image-col2 {
  width: 64mm;
  height: 46mm;
  object-fit: cover;
  border-radius: 10px;
}

.footer {
  background: #63c132;
  color: white;
  font-weight: bold;
  text-align: center;
  margin-top: auto;
  padding: 8px;
  border-radius: 50px;
  position: absolute;
  bottom: 5px;
  width: 85%;
  left: 50%;
  transform: translateX(-50%);
}

.export-btns {
  display: flex;
  color: blue;
  gap: 10px;
  justify-content: center;
  margin: 20px 0;
}
</style>
