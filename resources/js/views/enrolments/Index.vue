<template>
  <b-row align-h="center">
    <b-col cols="12">

      <b-table-simple responsive>
        <b-head>
          <b-tr>
            <b-th>
              Date
            </b-th>
            <b-th class="info2">
              Time
            </b-th>
            <b-th class="info2">
              Status
            </b-th>
            <b-th class="info2">
              Course
            </b-th>
            <b-th class="info">
              Lecturer
            </b-th>
            <b-th class="info3">
              Actions
            </b-th>
          </b-tr>
        </b-head>

        <body>
          <b-tr v-for="item in items" :key="item.id">
            <b-td>{{item.date}}</b-td>
            <b-td>{{item.time}}</b-td>
            <b-td>{{item.status}}</b-td>
            <b-td>{{item.course.title}}</b-td>
            <b-td>{{item.lecturer.name}}</b-td>
            <b-td>
              <router-link :to="`/enrolments/edit/${item.id}`">
              Edit
              </router-link>

            </b-td>
            <b-td>
              <router-link :to="`/enrolments/show/${item.id}`">
              <b-button variant="info">View</b-button>
              </router-link>
            </b-td>
            <b-td>
                  <b-button variant="danger" @click="deleteData(item)">Delete</b-button>

            </b-td>
          </b-tr>
        </body>
      </b-table-simple>

    </b-col>
  </b-row>
</template>

<script>
export default{
  data(){
    return {
      items: []
    }
  },
  created(){
    let app = this;
    let token = localStorage.getItem('token');
    axios.get('/api/enrolments',{
      headers: {Authorization: "Bearer " + token}
    })
    .then(function (response) {
        console.log(response.data.data);
       app.items = response.data.data;
    })
    .catch(function (error) {
       console.log(error);
    });
  },
  methods: {
   deleteData(item){

     let app = this;
     let token = localStorage.getItem('token');
     axios.delete('/api/enrolments/' + item.id, {
       headers: { Authorization: "Bearer " + token }
     })
     .then(function (response) {
       let idx = app.items.findIndex(function(obj){return obj.id == item.id})
       console.log(idx);
        app.items.splice(idx, 1);
        console.log(app.items);
     });
    }
  }
}
</script>

<style>
.info{
  padding-left:210px!important;
}
.info2{
  padding-left:60px!important;
}
.info3{
  padding-left:90px!important;
}
</style>
