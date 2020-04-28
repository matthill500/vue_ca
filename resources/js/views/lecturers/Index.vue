<template>
  <b-row align-h="center">
    <b-col cols="12">

      <b-table-simple responsive>
        <b-head>
          <b-tr class="info">
            <b-th>
              Name
            </b-th>
            <b-th class="info">
              Address
            </b-th>
            <b-th class="info3">
              Email
            </b-th>
            <b-th class="info3">
              Phone
            </b-th>
            <b-th class="info">
              Actions
            </b-th>
          </b-tr>
        </b-head>
      </br>
        <body>
          <b-tr v-for="lecturer in lecturers" :key="lecturer.id">
            <b-td>{{lecturer.name}}</b-td>
            <b-td>{{lecturer.address}}</b-td>
            <b-td>{{lecturer.email}}</b-td>
            <b-td>{{lecturer.phone}}</b-td>
            <b-td>
              <router-link :to="`/lecturers/edit/${lecturer.id}`">
              Edit
              </router-link>
            </b-td>
            <b-td>
              <router-link :to="`/lecturers/show/${lecturer.id}`">
              <b-button variant="info">View</b-button>
              </router-link>
            </b-td>
            <b-td>
                  <b-button variant="danger" @click="deleteData(lecturer)">Delete</b-button>
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
      lecturers:[]
    }
  },
  created(){
    let app = this;
    let token = localStorage.getItem('token');
    axios.get('/api/enrolments',{
      headers: {Authorization: "Bearer " + token}
    })
    .then(function (response) {
       app.enrolments = response.data.data;
       console.log(app.enrolments);
    })
    .catch(function (error) {
       console.log(error);
    });
    axios.get('/api/lecturers',{
      headers: {Authorization: "Bearer " + token}
    })
    .then(function (response) {
      app.lecturers = response.data.data;
       console.log(app.lecturers);

    })
    .catch(function (error) {
       console.log(error);
    });
  },
  methods: {
    deleteData(lecturer){
      let app = this;
      let token = localStorage.getItem('token');
      if(lecturer.enrolments.length > 1){
        alert("you cannot delete a lecturer with more than 1 enrollment!");
      }else if(lecturer.enrolments.length === 0){
        axios.delete('/api/lecturers/' + lecturer.id, {
          headers: { Authorization: "Bearer " + token }
        })
        .then(function (response){
          let idx = app.lecturers.findIndex(function(obj){return obj.id == lecturer.id})
          app.lecturers.splice(idx, 1);
        });
      }
      else{
        axios.delete('/api/enrolments/' + lecturer.enrolments[0].id, {
          headers: { Authorization: "Bearer " + token }
        })
        .then(function (response){
          axios.delete('/api/lecturers/' + lecturer.id, {
            headers: { Authorization: "Bearer " + token }
          })
          .then(function (response){
            let idx = app.lecturers.findIndex(function(obj){return obj.id == lecturer.id})
            app.lecturers.splice(idx, 1);
          });
        })
      }
    }
  }
}
</script>

<style>
.info{
  padding-left:150px!important;
}
.info2{
  margin-left:230px!important;
}
.info3{
  margin-left:530px!important;
}
</style>
