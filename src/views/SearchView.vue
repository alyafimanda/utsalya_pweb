<script>
import { ref } from "vue";
import axios from "axios";

export default {
  data() {
    return {
      cari: "",
      surat: ref([]),
      judul: ref([]),
      translations: ref([]),
      audio: "",
      name: [],
    };
  },

  watch: {
    cari() {
      this.getSurat();
      this.getJudul();
      this.getTerjemahan();
      this.getAudio();
    },
  },

  mounted() {
    this.getSurat();
    this.getJudul();
    this.getTerjemahan();
    this.getAudio();
  },

  methods: {
    getSurat() {
      axios
        .get(
          "https://api.quran.com/api/v4/quran/verses/uthmani?chapter_number=" +
            this.cari
        )
        .then((response) => {
          this.surat = response.data.verses;
        })
        .catch((error) => {
          console.log(error);
        });
    },
    getJudul() {
      axios
        .get(
          "https://api.quran.com/api/v4/chapters/" + this.cari + "?language=id"
        )
        .then((response) => {
          this.judul = response.data.chapter;
          this.name = this.judul.translated_name;
        })
        .catch((error) => {
          console.log(error);
        });
    },
    getAudio() {
      axios
        .get("https://api.quran.com/api/v4/chapter_recitations/4/" + this.cari)
        .then((response) => {
          this.audio = response.data.audio_file;
        })
        .catch((error) => {
          console.log(error);
        });
    },
    getTerjemahan() {
      axios
        .get(
          "https://api.quran.com/api/v4/quran/translations/39?chapter_number=" +
            this.cari
        )
        .then((response) => {
          this.translations = response.data.translations;
        })
        .catch((error) => {
          console.log(error);
        });
    },
  },
};
</script>

<template>
  <div class="text-center">
    <h2>Masukkan Nomor Surat</h2>
    <input
      v-model="cari"
      class="form-control me-2"
      type="search"
      placeholder="Masukkan Nomor Surat"
      aria-label="Search"
    />
    <div v-if="cari">
      <div class="mt-5">
        <h1>{{ judul?.name_complex }}</h1>
        <br />
        <h1>{{ name?.name }}</h1>
        <br />
        <h2>Diturunkan di {{ judul?.revelation_place }}</h2>
      </div>
      <p v-if="audio" class="text-lg-end mt-3">
        <audio v-bind:src="audio?.audio_url" controls></audio>
      </p>
    </div>
  </div>
  <div v-if="cari" v-for="(ayat, t) in surat" :key="t" class="card">
    <div class="card-body">
      <h5 class="card-title text-end">
        {{ ayat?.text_uthmani }} {{ ayat?.verse_key }}
      </h5>
      <p class="card-text text-center">Terjemahan :</p>
      <p class="card-text text-center">{{ translations[t]?.text }}</p>
    </div>
  </div>
</template>

<style scoped></style>
