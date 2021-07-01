<template>
  <div class="container">
    <div class="row no-gutters justify-content-center">
      <div class="col-lg-12 col-md-12 main-table">
        <h2 class="text-primary">List Of Users</h2>
        <p class="mt-3">Current Page: {{ currentPage }}</p>
        <div class="border user-data row no-gutters" id="my-table" v-for="(item, index) in userList" :per-page="perPage" :current-page="currentPage" small :key="index">
            <div class="col-md-4">
              <img class="user-image" :src="item.avatar" />
            </div>
            <div class="col-md-4 user-style text-left">
              {{item.first_name}} {{item.last_name}}
            </div>
            <div class="col user-style text-left">
              {{item.email}}
            </div>
        </div>
          <b-pagination
          v-model="currentPage"
          :total-rows="rows"
          :per-page="perPage"
          aria-controls="my-table"
          @input="changPage($event)"
        ></b-pagination>
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
 .user-data { 
   padding: 10px;
 }
 .user-image {
   height: 77px;
   border-radius: 50%;
 }
 .user-style {
   margin-top: auto;
   margin-bottom: auto;
 }
 .main-table{
    max-width: 70%;
    text-align: center;
    justify-content: center;
    margin: auto;
    box-shadow: 0px 30px 30px 0px rgb(0 11 40 / 10%);
    margin-bottom: 40px;
 }
 .pagination{
   justify-content: inherit;
 }
</style>
