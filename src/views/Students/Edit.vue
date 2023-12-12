<template>
    <div class="container mt-5">
        <div class="card">
            <div class="card-header">
                <h4>Edit Students</h4>
            </div>
            <div class="card-body">

                <ul class="alert alert-warning" v-if="Object.keys(this.errorList).length > 0">
                    <li class="mb-0 ms-3" v-for="(error, index) in this.errorList" :key="index">
                        {{ error[0] }}
                    </li>
                </ul>
                
                <div class="mb-3">
                    <label for="">Name</label>
                    <input type="text" v-model="model.student.name" class="form-control">
                </div>
                <div class="mb-3">
                    <label for="">Course</label>
                    <input type="text" v-model="model.student.course" class="form-control">
                </div>
                <div class="mb-3">
                    <label for="">Email</label>
                    <input type="text" v-model="model.student.email" class="form-control">
                </div>
                <div class="mb-3">
                    <label for="">Phone</label>
                    <input type="text" v-model="model.student.phone" class="form-control">
                </div>

                <div class="mb-3">
                    <button type="button" @click="updateStudent" class="btn btn-primary">Update</button>
                </div>

            </div>
        </div>
    </div>

</template>
<script>
import axios from 'axios';

export default{
    name: 'studentEdit',
    data(){
        return {
            errorList: {},
            model: {
                student: {
                    name: '',
                    course: '',
                    email: '',
                    phone: ''
                }
            }
        }
    },

    mounted(){
        var userId = this.$route.params.id;

        
        console.log(this.$route.params.id);
        this.getStudentData(userId);

    },

    methods:{

        getStudentData(studentId){
            axios.get('http://127.0.0.1:8000/api/students/' + studentId + '/edit').then(res=>{
                console.log(res);

                this.model.student= res.data.student
                //                  Можна І так
                // this.model.student.course = res.data.student.course 
                // this.model.student.email = res.data.student.email
                // this.model.student.phone = res.data.student.phone
                //
            })
            .catch(function (error) {
                if (error.response) {
                    if (error.response.status >= 404) {
                        alert(error.response.data.message)
                    }
                }
            });
        },

        updateStudent(){

            var mythis = this;
            axios.put('http://127.0.0.1:8000/api/students/' + this.$route.params.id + '/edit', this.model.student)
            .then(res => {
                console.log(res.data)
                alert(res.data.message);

               
                mythis.errorList = '';
            })
            .catch(function (error) {
                if (error.response) {
                    if (error.response.status >= 400 && error.response.status < 500) {
                        mythis.errorList = error.response.data.errors;
                    }

                    if (error.response.status == 404) {
                        alert(error.response.data.message);
                    }

                } else if (error.request) {
                    console.log(error.request);
                } else {
                    console.log('Error', error.message);
                }
            });

        }
    },
}
</script>