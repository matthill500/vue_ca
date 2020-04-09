<template>
  <b-row align-h="center">
    <b-col cols="8">
      <h3 v-if="!loggedIn">You are not logged in!!</h3>
      <b-card v-else title="Course" tag="article">
        <div>
          <ul>
            <li>Title: {{course.title}}</li>
            <li>Code: {{course.code}}</li>
            <li>Description: {{course.description}}</li>
            <li>Points: {{course.points}}</li>
            <li>Level: {{course.level}}</li>
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
        course: {},
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
      axios.get(`/api/courses/${app.$route.params.id}`, {
        headers: { Authorization: "Bearer " + token }
      })
      .then(function (response) {
        app.course = response.data.data;
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
