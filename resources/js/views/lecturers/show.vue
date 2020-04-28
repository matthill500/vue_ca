<template>
  <b-row align-h="center">
    <b-col cols="8">
      <h3 v-if="!loggedIn">You are not logged in!!</h3>
      <b-card v-else title="Lecturer" tag="article">
        <div>
          <ul>
            <li>Name: {{lecturer.name}}</li>
            <li>Address: {{lecturer.address}}</li>
            <li>phone: {{lecturer.phone}}</li>
            <li>email: {{lecturer.email}}</li>
          </ul>
        </div>
      </b-card>
    </b-col>
  </b-row>
</template>

<script>
  export default {
    data() {
      return {
        lecturer: {},
        show: true,
        loggedIn: false
      }
    },
    created() {
      if (localStorage.getItem('token')) {
        this.loggedIn = true;
      }
      else {
        this.loggedIn = false;
      }

      let app = this;
      let token = localStorage.getItem('token');
      axios.get(`/api/lecturers/${app.$route.params.id}`, {
        headers: { Authorization: "Bearer " + token }
      })
      .then(function (response) {
        app.lecturer = response.data.data;
        console.log(app.lecturer);
      })
      .catch(function (error) {
        console.log(error);
      });
    },
        methods:{

        }

  }
</script>
