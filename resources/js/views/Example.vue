<template>
  <div>
    Test Login <br>
    <button @click="login()">Login</button>
    <br>
    Test Logout <br>
    <button @click="logout()">Logout</button>
    <br>
    Test courses <br>
    <button @click="getCourses()"> Get Courses </button>
    <br>
    Test lecturers <br>
    <button @click="getLecturers()"> Test Lecturers </button>

  </div>
</template>
<script>
export default {
  name: 'example',
  components: {

  },
  data() {
    return {
      email: "sam@bloggs.com",
      password: "secret",
      name: "",
      courses: []
    }
  },
    mounted() {
      if (localStorage.getItem('token') !== null){
        console.log('***USER LOGGED IN***');
      }else{
        console.log('***USER LOGGED OUT***');
      }
    },
  methods: {
    login() {
      let app = this;
      axios.post('/api/login', {
        email: app.email,
        password: app.password
      })
      .then(function (response) {
         console.log(response.data);
         app.name = response.data.name;
         localStorage.setItem('token', response.data.token);
      })
      .catch(function (error) {
         console.log(error);
      });
    },
    getCourses() {
      let app = this;
      let token = localStorage.getItem('token');
      axios.get('/api/courses',{
        headers: {Authorization: "Bearer " + token}
      })
      .then(function (response) {
         console.log(response.data.data);
         app.courses = response.data.data;
      })
      .catch(function (error) {
         console.log(error);
      });
    },
    getLecturers() {
      let app = this;
      let token = localStorage.getItem('token');
      axios.get('/api/lecturers',{
        headers: {Authorization: "Bearer " + token}
      })
      .then(function (response) {
         console.log(response.data.data);
         app.courses = response.data.data;
      })
      .catch(function (error) {
         console.log(error);
      });
    },
    logout() {
      localStorage.removeItem('token');
      console.log("logged out");
    }
  }
}


</script>
<style>
</style>
