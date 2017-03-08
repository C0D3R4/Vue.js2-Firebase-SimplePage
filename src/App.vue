<template>
  <div id="app" class="container">
    <div class="page-header">
      <h1>Vue.JS 2 & Firebase simple page application</h1>
    </div>

    <div class="panel panel-default">
        <div class="panel-heading">
          <h3>Add Course</h3>
        </div>
          <div class="panel-body">
            <form id="form" class="form-inline" v-on:submit.prevent="addCourse">
              <div class="form-group">
                <label for="CourseTittle">Title:</label>
                <input type="text" id="CourseTitle" class="form-control" v-model="newCourse.Title">
              </div>
              <div class="form-group">
                <label for="CourseAuthor">Author:</label>
                <input type="text" id="CourseAuthor" class="form-control" v-model="newCourse.Author">
              </div>
              <div class="form-group">
                <label for="CourseLink">Link:</label>
                <input type="text" id="CourseLink" class="form-control" v-model="newCourse.Link">
              </div>
              <input type="submit" class="btn btn-primary" value="Add Course">
            </form>
          </div>
    </div>


    <div class="panel panel-default">
      <div class="panel-heading">
        <h3>Courses Lists</h3>
      </div>
      <div class="panel-body">
        <table class="table table-striped">
            <thead>
              <tr>
                <th>Title</th>
                <th>Author</th>
                <th>Delete</th>
              </tr>
            </thead>
            <tbody>
              <tr v-for="course in Courses">
                <td>
                  <a v-bind:href="course.Link">{{course.Title}}</a>
                </td>
                <td>
                  {{course.Author}}
                </td>
                <td>
                  <span class="glyphicon glyphicon-trash"v-on:click="removeCourse(course)"></span>
                </td>
              </tr>
            </tbody>
        </table>
      </div>
    </div>
  </div>
</template>

<script>
import Firebase from 'firebase'

import toastr from 'toastr'

let config = {
  apiKey: "AIzaSyCETV4YYBUAiBM2ZpYc_s4YzDZ6HWxoRh8",
  authDomain: "vuejs-2-firebase.firebaseapp.com",
  databaseURL: "https://vuejs-2-firebase.firebaseio.com",
  storageBucket: "vuejs-2-firebase.appspot.com",
  messagingSenderId: "552503647438"
}

let app = Firebase.initializeApp(config);
let db = app.database();
let courseRef = db.ref('Courses');
export default {
  name: 'app',
  firebase: {
    Courses: courseRef
  },
  data () {
    return {
      newCourse: {
        Title: '',
        Author: '',
        Link: ''
      }
    }
  },
  methods: {
    addCourse: function addCourse() {
      courseRef.push(this.newCourse)
      this.newCourse.Title='';
      this.newCourse.Author='';
      this.newCourse.Link='';
    },
    removeCourse: function(course){
      courseRef.child(course['.key']).remove();
      toastr.success("Course Removed");
    }
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
