<template>
    <main>
        <div class="card">
            <div class="card-header">
                <h4>
                    Students
                    <RouterLink to="/student/create" class="btn btn-primary float-end">Add Student</RouterLink>
                </h4>
            </div>
            <div class="card-body">
                <table class="table table-bordered">
                    <thead>
                        <tr>
                            <th>Id</th>
                            <th>Name</th>
                            <th>Course</th>
                            <th>Email</th>
                            <th>Phone</th>
                            <th>Create At</th>
                            <th>Action</th>
                        </tr>
                    </thead>
                    <tbody v-if="this.students.length > 0">
                        <tr v-for="(student, index) in this.students" :key="index">
                            <td>{{ student.id }}</td>
                            <td>{{ student.name }}</td>
                            <td>{{ student.course }}</td>
                            <td>{{ student.email }}</td>
                            <td>{{ student.phone }}</td>
                            <td>{{ student.created_at }}</td>
                            <td>
                                <RouterLink class="btn btn-primary" :to="{ name:'studentEdit', params: { id: student.id }}">Edit</RouterLink>
                                <button class="btn btn-danger" @click="deleteStudent(student.id)">Delete</button>
                            </td>
                        </tr>
                    </tbody>
                    <tbody v-else>
                        <tr>
                            <td colspan="7">Loading...</td>
                        </tr>
                    </tbody>
                </table>
            </div>
        </div>
    </main>
</template>
<script>
import { RouterLink } from 'vue-router'
import axios from 'axios'

export default {
    name: 'Students',
    data() {
        return {
            students: []
        }
    },
    mounted() {
        this.getStudents()
    },
    methods: {
     getStudents() {
        axios.get('http://localhost:8000/api/students')
      .then(response => {
        this.students = response.data.students;
      })
     },
     deleteStudent(id) { 
        axios.delete('http://localhost:8000/api/students/' + id)
      .then(response => {
        if (confirm('Are you sure, you want to delete this data ?')) {
            alert(response.data.message)
            this.getStudents()
        }
      })
      .catch(function(error) {
        if (error.response) {
            if (error.response.status == 404) {
                alert(error.response.data.message)
            }
        }
      })
     }
    }
}
</script>