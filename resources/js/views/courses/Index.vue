<template>
  <b-row align-h="center">
    <b-col cols="12">

      <b-table-simple responsive>
        <b-head>
          <b-tr>
            <b-th>
              Title
            </b-th>
            <b-th>
              Code
            </b-th>
            <b-th>
              Description
            </b-th>
            <b-th>
              Points
            </b-th>
            <b-th>
              Level
            </b-th>
            <b-th>
              Actions
            </b-th>
          </b-tr>
        </b-head>

        <body>
          <b-tr v-for="item in items" :key="item.id">
            <b-td>{{item.title}}</b-td>
            <b-td>{{item.code}}</b-td>
            <b-td>{{item.description}}</b-td>
            <b-td>{{item.points}}</b-td>
            <b-td>{{item.level}}</b-td>
            <b-td>
              <router-link :to="`/items/edit/${item.id}`">
              Edit
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
      items:[]
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
    axios.get('/api/courses',{
      headers: {Authorization: "Bearer " + token}
    })
    .then(function (response) {

       app.items = response.data.data;
        console.log(app.items);
    })
    .catch(function (error) {
       console.log(error);
    });
  },
  methods:{
    deleteData(item){
      let app = this;
      let token = localStorage.getItem('token');
      if(item.enrolments.length > 1){
        alert("you cannot delete a course with more than 1 enrolment!");
      }else if(item.enrolments.length === 0){
        axios.delete('/api/courses/' + item.id, {
          headers: { Authorization: "Bearer " + token }
        })
        .then(function (response){
          let idx = app.items.findIndex(function(obj){return obj.id == item.id})
          app.items.splice(idx, 1);

        });
      }else{
        axios.delete('/api/enrolments/' + item.enrolments[0].id, {
          headers: { Authorization: "Bearer " + token }
        })
        .then(function (response){
          console.log(item);
          axios.delete('/api/courses/' + item.id, {
            headers: { Authorization: "Bearer " + token }
          })
          .then(function (response){
            let idx = app.items.findIndex(function(obj){return obj.id == item.id})
            app.items.splice(idx, 1);
          });
        })
      }
    }
  }
}

</script>

<style>

</style>
