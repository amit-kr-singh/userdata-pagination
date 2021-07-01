<template>
  <div class="container">
    <div class="row no-gutters justify-content-center">
      <h2 class="text-primary">List Of Users</h2>
      <div class="overflow-auto">
        <b-pagination
          v-model="currentPage"
          :total-rows="rows"
          :per-page="perPage"
          aria-controls="my-table"
          @input="changPage($event)"
        ></b-pagination>

        <p class="mt-3">Current Page: {{ currentPage }}</p>

        <b-table
          id="my-table"
          :items="userList"
          :per-page="perPage"
          :current-page="currentPage"
          small
        ></b-table>
          <!-- ref="table" -->
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "UserData",

  data() {
    return {
      userList: [],
      currentPage: 1,
      perPage: 6,
      rows: 12
    };
  },

  methods: {
    getUser() {
      fetch(`https://reqres.in/api/users?page=${this.currentPage}`)
        .then(response => {
          return response.json();
        })
        .then(data => {
          // console.log(data);
          this.userList = data.data;
          this.currentPage = data.page;
          // console.log("Data", data);
        });
    },
    changPage(event) {
      fetch(`https://reqres.in/api/users?page=${event}`)
        .then(response => {
          return response.json();
        }).then(data => {
          // console.log(data.data.length);
          this.$nextTick(()=>{
            console.log(data)
            this.userList = []
            this.userList = data.data;
            // console.log(this.userList);
            this.currentPage = data.page;
          })
          // this.$refs.table.refresh();
          // this.$forceUpdate();
          // console.log("Data", data);
        });
    }
  },

  created() {
    this.getUser();
  }
};
</script>

<style>
</style>
