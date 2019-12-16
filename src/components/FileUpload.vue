<template>
  <b-row class="m-3">
    <b-col sm="6">
      <b-form-file
        class="mt-2"
        multiple
        v-model="files"
        :state="Boolean(files)"
        placeholder="Choose a file or drop it here..."
        drop-placeholder="Drop file here..."
      ></b-form-file>
      <div class="mt-3">
        Selected file:
        <b-list-group class="sm">
          <b-list-group-item v-for="file in files" v-bind:key="file.name" class="sm">{{ file.name }}</b-list-group-item>
        </b-list-group>
      </div>
      <b-button variant="primary" class="float-right mt-2" @click="upload">Upload</b-button>
    </b-col>
    <b-col>
      <div>
        <b-tabs content-class="mt-3">
          <b-tab title="A-G" active>
            <b-table striped hover :items="aGitems" responsive id="a-g-table" :per-page="perPage" :current-page="aGcurrentPage" :fields="fields"></b-table>
            <b-pagination
              v-model="aGcurrentPage"
              :total-rows="aGrows"
              :per-page="perPage"
              aria-controls="a-g-table"
            ></b-pagination>
          </b-tab>
          <b-tab title="H-N">
            <b-table striped hover :items="hNitems" responsive id="h-n-table" :per-page="perPage" :current-page="hNcurrentPage" :fields="fields"></b-table>
            <b-pagination
              v-model="hNcurrentPage"
              :total-rows="hNrows"
              :per-page="perPage"
              aria-controls="h-n-table"
            ></b-pagination>
          </b-tab>
          <b-tab title="O-U">
            <b-table striped hover :items="oUitems" responsive id="o-u-table" :per-page="perPage" :current-page="oUcurrentPage" :fields="fields"></b-table>
            <b-pagination
              v-model="oUcurrentPage"
              :total-rows="oUrows"
              :per-page="perPage"
              aria-controls="o-u-table"
            ></b-pagination>
          </b-tab>
          <b-tab title="V-Z">
            <b-table
              striped
              hover
              :items="vZitems"
              responsive
              id="v-z-table"
              :per-page="perPage"
              :current-page="vZcurrentPage"
              :fields="fields"
            ></b-table>
            <b-pagination
              v-model="vZcurrentPage"
              :total-rows="vZrows"
              :per-page="perPage"
              aria-controls="v-z-table"
            ></b-pagination>
          </b-tab>
        </b-tabs>
      </div>
    </b-col>
  </b-row>
</template>

<script>
import axios from "axios";
export default {
  name: "FileUpload",
  data: function() {
    return {
      files: null,
      perPage: 20,
      aGitems: null,
      hNitems: null,
      oUitems: null,
      vZitems: null,
      fields: [
        { key: "word", sortable: true },
        { key: "count", sortable: true }
      ],
      aGcurrentPage: 1,
      hNcurrentPage: 1,
      oUcurrentPage: 1,
      vZcurrentPage: 1,
      aGrows: 0,
      hNrows: 0,
      oUrows: 0,
      vZrows: 0
    };
  },
  methods: {
    upload: function() {
      let thisRef = this;
      let formData = new FormData();
      this.files.forEach(file => formData.append("files", file));

      axios
        .post(process.env.VUE_APP_BACKEND_URL + "/vocabulary", formData, {
          headers: {
            "Content-Type": "multipart/form-data",
          }
        })
        .then(function(data) {
          
          thisRef.aGitems = data.data['A-G'];
          thisRef.aGrows = thisRef.aGitems.length;
          thisRef.hNitems = data.data['H-N'];
          thisRef.hNrows = thisRef.hNitems.length;
          thisRef.oUitems = data.data['O-U'];
          thisRef.oUrows = thisRef.oUitems.length;
          thisRef.vZitems = data.data['V-Z'];
          thisRef.aGrows = thisRef.vZitems.length;
        })
        .catch(function() {
          console.log("FAILURE!!");
        });
    }
  }
};
</script>

