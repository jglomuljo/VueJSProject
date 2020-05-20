<template>
    <div>

        <div>
            <center>
            <img src="/img/head.png" width="500px">
            </center><br>
        </div>

        <div v-if="!loading">
            <img class="rounder mx-auto d-block" :src="image" alt="loader" width="300px">
        </div>

        <div v-else>

        <button @click= "createModal" class="btn btn-block" style="color:white; background-color:rgb(26,165,91)">Add New Patient</button>
        <table class="table table-striped" v-if="patients">
            <thead>
                <tr>
                    <th scope="col">id</th>
                    <th scope="col">Name</th>
                    <th scope="col">Location</th>
                    <th scope="col">Status</th>
                </tr>
            </thead>
            <tbody>
                <tr v-for="(patient, index) in patients">
                    <td>{{ index + 1 }}</td>
                    <td>{{ patient.name }}</td>
                    <td>{{ patient.location }}</td>
                    <td v-bind:style="myStyle" class ="status">{{ patient.status }}</td>
                    <td><button @click = "updateModal(index)" class="btn btn-outline-info">Edit</button></td>
                    <td><button @click = "deletePatient(index)" class="btn btn-outline-danger">Delete</button></td>
                </tr>
            </tbody>
        </table>

        <!-- Modal -->
        <div class="modal fade" id="create-modal" tabindex="-1" role="dialog" aria-labelledby="exampleModalLabel" aria-hidden="true">
        <div class="modal-dialog" role="document">
            <div class="modal-content">
            <div class="modal-header">
                <h5 class="modal-title" id="exampleModalLabel">Add New Patient</h5>
                <button type="button" class="close" data-dismiss="modal" aria-label="Close">
                <span aria-hidden="true">&times;</span>
                </button>
            </div>
            <div class="modal-body">
                <div class="alert alert-danger" v-if="errors.length > 0"> 
                    <ul>
                        <li v-for="error in errors">{{error}}</li>
                    </ul>
                </div>
                <div class="form-group">
                    <label for="name">Name</label>
                    <input v-model ="patient.name" type="text" id="name" class="form-control">
                </div>
                <div class="form-group">
                    <label for="location">Location</label>
                    <input v-model ="patient.location" type="text" id="location" class="form-control">
                </div>
                
        <!-- radios -->

                <label for="location">Status</label><br>
            <div class ="card">
                <div class="card-body m-2">
                    <table>
                            <tr>
                                <td>
                                    <input type="radio" id="positive" value="Positive" v-model="patient.status" style="margin: 15px">
                                    <label for="positive">Postive</label>
                                </td>
                                <td>
                                    <input type="radio" id="negative" value="Negative" v-model="patient.status" style="margin: 15px">
                                    <label for="negative">Negative</label>
                                </td>
                                <td>
                                    <input type="radio" id="deceased" value="Deceased" v-model="patient.status" style="margin: 15px">
                                    <label for="deceased">Deceased</label><br>
                                </td>
                            </tr>

                            <tr>
                                <td>
                                    <input type="radio" id="actpositive" value="Actively Positive" v-model="patient.status" style="margin: 15px">
                                    <label for="actpositive">Actively Postive</label>
                                </td>
                                <td>
                                    <input type="radio" id="recovered" value="Recovered" v-model="patient.status" style="margin: 15px">
                                    <label for="recovered">Recovered</label>
                                </td>
                            </tr>
                    </table><br>
                    <span>Picked: {{ patient.status }}</span>
                </div>
            </div>

            </div>
            <div class="modal-footer">
                <button type="button" class="btn btn-outline-secondary" data-dismiss="modal">Close</button>
                <button @click= "createPatient" type="button" class="btn btn-outline-primary">Save changes</button>
            </div>
            </div>
        </div>
        </div>
        <!-- end of modal -->

            <!-- Modal -->
            <div class="modal fade" id="update-modal" tabindex="-1" role="dialog" aria-labelledby="exampleModalLabel" aria-hidden="true">
            <div class="modal-dialog" role="document">
                <div class="modal-content">
                <div class="modal-header">
                    <h5 class="modal-title" id="exampleModalLabel">Edit Patient</h5>
                    <button type="button" class="close" data-dismiss="modal" aria-label="Close">
                    <span aria-hidden="true">&times;</span>
                    </button>
                </div>
                <div class="modal-body">
                    <div class="alert alert-danger" v-if="errors.length > 0"> 
                        <ul>
                            <li v-for="error in errors">{{error}}</li>
                        </ul>
                    </div>
                    <div class="form-group">
                        <label for="name">Name</label>
                        <input v-model ="new_update_patient.name" type="text" id="name" class="form-control">
                    </div>
                    <div class="form-group">
                        <label for="location">Location</label>
                        <input v-model ="new_update_patient.location" type="text" id="location" class="form-control">
                    </div>

                    <!-- radios -->
                    <label for="location">Status</label><br>
                <div class ="card">
                    <div class="card-body m-2">
                        <table>
                                <tr>
                                    <td>
                                        <input type="radio" id="positive" value="Positive" v-model="new_update_patient.status" style="margin: 15px">
                                        <label for="positive">Postive</label>
                                    </td>
                                    <td>
                                    <input type="radio" id="negative" value="Negative" v-model="new_update_patient.status" style="margin: 15px">
                                        <label for="negative">Negative</label>
                                    </td>
                                    <td>
                                    <input type="radio" id="deceased" value="Deceased" v-model="new_update_patient.status" style="margin: 15px">
                                    <label for="deceased">Deceased</label><br>
                                    </td>
                                </tr>

                                <tr>
                                    <td>
                                    <input type="radio" id="actpositive" value="Actively Positive" v-model="new_update_patient.status" style="margin: 15px">
                                    <label for="actpositive">Actively Postive</label>
                                    </td>
                                    <td>
                                    <input type="radio" id="recovered" value="Recovered" v-model="new_update_patient.status" style="margin: 15px">
                                    <label for="recovered">Recovered</label>
                                    </td>
                                </tr>
                        </table><br>
                        <span>Picked: {{ new_update_patient.status }}</span>

                    </div>
                </div>

                </div>
                <div class="modal-footer">
                    <button type="button" class="btn btn-outline-secondary" data-dismiss="modal">Close</button>
                    <button @click= "updatePatient" type="button" class="btn btn-outline-primary">Save changes</button>
                </div>
                </div>
            </div>
            </div>
            <!-- end of modal -->

        <div class="card">
            <div class="card-body">
                <div class="container">
                     <center>Legends</center><br>
                     <center>
                        <table id="butts">
                                <tr>
                                    <td>
                                        <button type="button" class="btn" style="color:white;background-color:orange " disabled>Positive</button>
                                    </td>
                                    <td>
                                        <button type="button" class="btn btn-danger" disabled>Actively Positive</button>
                                    </td>
                                    <td>
                                        <button type="button" class="btn btn-primary" disabled >Negative</button>
                                    </td>
                                    <td>
                                        <button type="button" class="btn btn-success" disabled >Recovered</button>
                                    </td>
                                    <td>
                                        <button type="button" class="btn btn-secondary" disabled >Deceased</button>
                                    </td>
                                </tr>
                        </table>
                    </center>
                </div>
            </div>
        </div>


</div>
</div>

</template>

<script>

            // $("#butts tr td button").hover(
            //     function(){
            //     var age = $(".status").text();
            //     if (age == "Recovered"){
            //         $(".status").css("background-color", "green");
            //         }
            //     if (age == "Positive"){
            //         $(".status").css("background-color", "orange");
            //         }
            //     if (age == "Negative"){
            //         $(".status").css("background-color", "blue");
            //         }
            //     if (age == "Actively Positive"){
            //         $(".status").css("background-color", "red");
            //         }
            //     if (age == "Deceased"){
            //         $(".status").css("background-color", "gray");
            //         }

            //     }, 
            //     function(){
            //         $(".status").css("background-color", "white");
            //     })


    export default {

        data(){
            return{
                patient:{
                    name: "",
                    location: "",
                    status: ""
                },

                patients: [],
                errors: [],
                new_update_patient: [],
                uri: 'http://127.0.0.1:8000/patients/',
                image: 'img/loader.gif',
                loading: false,

                picked: ''
            }

            // myStyle:{
            //     function highLite(){
            //         $("#butts tr td button").hover(
            //         var age = $(".status").text();
            //             if (age == "Recovered"){
            //                 $(".status").css("background-color", "green");
            //                 }
            //             if (age == "Positive"){
            //                 $(".status").css("background-color", "orange");
            //                 }
            //             if (age == "Negative"){
            //                 $(".status").css("background-color", "blue");
            //                 }
            //             if (age == "Actively Positive"){
            //                 $(".status").css("background-color", "red");
            //                 }
            //             if (age == "Deceased"){
            //                 $(".status").css("background-color", "gray");
            //                 }

            //     }, 
            //     function(){
            //         $(".status").css("background-color", "white");
            //     })
            // }
            
        },

        methods:{

            createModal(){
                $("#create-modal").modal("show");
            },

            updateModal(index){
                this.errors = [];
                $("#update-modal").modal("show");
                this.new_update_patient = this.patients[index];
            },

            createPatient(){
                axios.post(this.uri, {name: this.patient.name, location: this.patient.location, status: this.patient.status})
                .then(response=>{

                    this.resetData();
                    this.patients.push(response.data.patient);
                    $("#create-modal").modal("hide");
                    toastr.success(response.data.message);
                })

                .catch(error=>{
                    this.errors = [];
                    if (error.response.data.errors.name){
                        this.errors.push(error.response.data.errors.name[0]);
                    }
                    if (error.response.data.errors.location){
                        this.errors.push(error.response.data.errors.location[0]);
                    }
                    if (error.response.data.errors.status){
                        this.errors.push(error.response.data.errors.status[0]);
                    }
                    
                })
            },

            updatePatient(){
                axios
                .patch(this.uri + this.new_update_patient.id, {
                    name: this.new_update_patient.name, 
                    location: this.new_update_patient.location, 
                    status: this.new_update_patient.status
                })
                .then(response=>{
                    $("#update-modal").modal("hide");
                    toastr.success(response.data.message);
                })

                .catch(error=>{
                    this.errors = [];
                    if (error.response.data.errors.name){
                        this.errors.push(error.response.data.errors.name[0]);
                    }
                    if (error.response.data.errors.location){
                        this.errors.push(error.response.data.errors.location[0]);
                    }
                    if (error.response.data.errors.status){
                        this.errors.push(error.response.data.errors.status[0]);
                    }
                    
                })
            },
            
            deletePatient(index){
                let confirmBox = confirm("Do you want to delete Patient and Patient Data?");

                if(confirmBox == true){
                    axios.delete(this.uri + this.patients[index].id)
                    .then(response=>{
                        this.$delete(this.patients, index);
                        toastr.success(response.data.message);
                    })
                    .catch(error => {
                        console.log("Patient could not be deleted.")
                    });
                }
            },

            loadPatient(){

                axios.get(this.uri).then(response=> {
                    this.patients = response.data.patients;
                    this.loading = true;
                });
            },

            resetData(){
                this.patient.name = '';
                this.patient.location = '';
                this.patient.status = '';
            }
        },

        mounted() {
            this.loadPatient();
            
        }
    }
</script>