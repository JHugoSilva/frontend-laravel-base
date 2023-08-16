<template>
    <div class="container mt-5">
        <div class="card">
                <div class="card-header">
                    <h4>Edit Student</h4>
                </div>
                <div class="card-body">
                    <ul class="alert alert-warning" v-if="Object.keys(this.errorList).length > 0">
                        <li class="mb-0 ms-3" v-for="(error, index) in this.errorList" :key="index">
                            {{ error[0] }}
                        </li>
                    </ul>
                    <div class="mb-3">
                        <label for="">Name</label>
                        <input type="text" class="form-control" v-model="model.student.name">
                    </div>
                    <div class="mb-3">
                        <label for="">Course</label>
                        <input type="text" class="form-control" v-model="model.student.course">
                    </div>
                    <div class="mb-3">
                        <label for="">Email</label>
                        <input type="text" class="form-control" v-model="model.student.email">
                    </div>
                    <div class="mb-3">
                        <label for="">Phone</label>
                        <input type="text" class="form-control" v-model="model.student.phone">
                    </div>
                    <div class="mb-3">
                        <button type="button" class="btn btn-primary" @click="updateStudent">Save</button>
                    </div>
                </div>
        </div>
    </div>
</template>

<script>
import axios from 'axios'

export default {
    name: 'studentEdit',
    data(){
        return{
            errorList: '',
            studentId: '',
            model:{
                student: {
                    name:'',
                    course:'',
                    email:'',
                    phone:'',
                }
            }
        }
    },
    methods:{
        getStudentData() {
            axios.get(`http://localhost:8000/api/students/${this.studentId}/edit`)
            .then(response => {
                this.model.student = response.data.student
            })
            .catch(function(error){
                if (error.response) {
                    if (error.response.status == 404) {
                        alert(error.response.data.message)
                    }
                }
            })
        },
        updateStudent(){
            var mythis = this
            axios.put(`http://localhost:8000/api/students/${this.studentId}`, this.model.student)
          .then(response => {
                alert(response.data.message)
                this.errorList = ''
            })
          .catch(error => {
                if (error.response) {
                    if (error.response.status == 422) {
                        mythis.errorList = error.response.data.message
                    }
                    if (error.response.status == 404) {
                        mythis.errorList = error.response.data.message
                    }
                } else if (error.request) {
                    console.log(error.request)
                } else {
                    console.log(error.request)
                }
            })
        }
    },
    mounted() {
        this.studentId = this.$route.params.id
        this.getStudentData(this.studentId)
    }
}
</script>