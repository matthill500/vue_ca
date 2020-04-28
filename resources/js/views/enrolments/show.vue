<template>
  <b-row align-h="center">
    <b-col cols="8">
      <h3 v-if="!loggedIn">You are not logged in!!</h3>
      <b-card v-else title="Enrolment" tag="article">
        <div>
          <ul>
            <li>Date: {{enrolment.date}}</li>
            <li>Time: {{enrolment.time}}</li>
            <li>Status: {{enrolment.status}}</li>
            <li>Course: {{enrolment.course.title}}</li>
            <li>Lecturer: {{enrolment.lecturer.name}}</li>
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
        enrolment: {},
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
      axios.get(`/api/enrolments/${app.$route.params.id}`, {
        headers: { Authorization: "Bearer " + token }
      })
      .then(function (response) {
        app.enrolment = response.data.data;
        console.log(app.enrolment);
      })
      .catch(function (error) {
        console.log(error);
      });
    },
        methods:{

        }

  }
</script>
