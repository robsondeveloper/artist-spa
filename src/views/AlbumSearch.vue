<template>
  <div>
    <b-input-group>
      <b-form-input
        id="search"
        type="search"
        v-model="search"
        placeholder="nome do artista..."
      >
      </b-form-input>
      <b-input-group-append>
        <b-button @click="searchByArtistName" v-b-tooltip title="Pesquisar">
          <b-icon icon="search"></b-icon>
        </b-button>
      </b-input-group-append>
    </b-input-group>

    <b-table :items="albums" :fields="fields" caption-top>
      <template v-slot:cell(actions)="row">
        <router-link
          :to="{
            name: 'AlbumDetail',
            params: { album: row.item, search: search }
          }"
        >
          Detalhar
        </router-link>
        |
        <router-link
          :to="{
            name: 'AlbumCover',
            params: { album: row.item, search: search }
          }"
        >
          Capas
        </router-link>
      </template>
      <template #table-caption>
        <span class="font-weight-bold display-4">Álbuns</span>
      </template>
    </b-table>
  </div>
</template>

<script>
import AlbumService from "@/services/AlbumService";
import Swal from "sweetalert2";

export default {
  data() {
    return {
      search: "",
      albums: [],
      fields: [
        { key: "name", label: "Nome" },
        { key: "artist.name", label: "Artista" },
        { key: "actions", label: "" }
      ]
    };
  },
  created() {
    if ("search" in this.$route.query) {
      this.search = this.$route.query.search;
      this.searchByArtistName();
    }
  },
  methods: {
    searchByArtistName() {
      AlbumService.search(this.search).then(response => {
        this.albums = response.data;
        if (!this.albums.length) {
          Swal.fire("", "A pesquisa não retornou nenhum Álbum.", "info");
        }
      });
    }
  }
};
</script>

<style></style>
