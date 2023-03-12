<template>
    <div class="container mt-5">
        <div class="card">
            <div class="card-header">
                <h4>Add Students</h4>
            </div>
            <div class="card-body">
                <ul class="alert alert-warning" v-if="object.keys(this.errorList).length > 0">
                    <li class="mb-0 ms-3" v-for="(error, index) in this.errorList" :key="index">
                       {{ error[0] }}
                    </li>
                </ul>
                <div class="mb-3">
                    <label for="">id</label>
                    <input type="text" v-model="model.student.id" class="form-control"/>
                </div>
                <div class="mb-3">
                    <label for="">Name</label>
                    <input type="text" v-model="model.student.name" class="form-control"/>
                </div>
                <div class="mb-3">
                    <label for="">Active</label>
                    <input type="text" v-model="model.student.active" class="form-control"/>
                </div>
                <div class="mb-3">
                    <label for="">Years Old</label>
                    <input type="text" v-model="model.student.yearsOld" class="form-control"/>
                </div>
                <div class="mb-3">
                    <label for="">Color</label>
                    <input type="text" v-model="model.student.color" class="form-control"/>
                </div>
                <div class="mb-3">
                    <button type="button" @click="saveStudent" class="btn btn-primary">Save</button>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
import axios from 'axios';
export default {
    name:'studentCreate',
    data(){
        return {
        errorList:'',
        model: {
            student:{
                id : '',
                name: '',
                active:'',
                yearsOld:'',
                color:''
            }
        }
    }
},
methods: {
    saveStudent(){
        var $mythis = this;
        axios.post('http://localhost:8000/api/students', this.model.student)
        .then(res => {
        console.log(res.data)
        alert(res.data.message);
        this.model.student = {
                id : '',
                name: '',
                active:'',
                yearsOld:'',
                color:''
        }
        })
        .catch(function(error){
            if(error.response){
                if(error.response.status == 422){
                    $mythis.errorList = error.response.data.errors;
                }
                console.log(error.response.data);
                console.log(error.response.status);
                console.log(error.response.headers);
            } else if (error.request){
                console.log(error.request);
            }else {
                console.log('Error', error.message);
            }
        });
    }
}
}
</script>