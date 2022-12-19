<template>
   <div class="container mt-3">
    <div class="row">
        <div class="col">
            <p class="h3 text-success fw-bold">Add Contact</p>
            <p class="fst-italic">Lorem ipsum dolor sit amet consectetur adipisicing elit. Voluptatum quos labore hic? Quisquam vero, illo odio commodi perspiciatis veniam suscipit iure temporibus, nobis natus vel cum recusandae. Debitis, provident deleniti?</p>
        </div>
    </div>
   </div>

   <div class="container mt-3">
        <div class="row">
            <div class="col-md-4">
                <form @submit.prevent="submitCreate()">
                    <div class="mb-2">
                        <input required type="text" v-model="contact.name" class="form-control" placeholder="Name">
                    </div>
                    <div class="mb-2">
                        <input required type="text" v-model="contact.photo" class="form-control" placeholder="Photo URL">
                    </div>
                    <div class="mb-2">
                        <input required type="email" v-model="contact.email" class="form-control" placeholder="Email">
                    </div>
                    <div class="mb-2">
                        <input required type="number" v-model="contact.mobile" class="form-control" placeholder="Mobile">
                    </div>
                    <div class="mb-2">
                        <input required type="text" v-model="contact.company" class="form-control" placeholder="Company">
                    </div>
                    <div class="mb-2">
                        <input required type="text" v-model="contact.title" class="form-control" placeholder="Title">
                    </div>
                    <div class="mb-2">
                        <select required v-model="contact.groupId" class="form-control" v-if="groups.length > 0">
                            <option value="">Select Group</option>
                            <option :value="group.id" v-for="group of groups" :key="group.id">{{ group.name }}</option>
                        </select>
                    </div>
                    <div class="mb-2">
                        <input type="submit" class="btn btn-success" value="Create">
                    </div>
            </form>
            </div>

            <div class="col-md-4">
                <img class="contact-img" :src="contact.photo" alt="">
            </div>
        </div>
   </div>
    </template>
        
        
    <script>
import { ContactService } from '@/services/ContactService';

    export default{
        name: 'AddContact',
        data: function (){
            return {
                contact: {
                    name: '',
                    photo:'',
                    email:'',
                    mobile:'',
                    company:'',
                    title:'',
                    groupId:''
                }, 
                groups: []
            }
        },
        created : async function (){
         try{
            let response = await ContactService.getAllGroups();
            this.groups = response.data;
         }
         catch(error){
            console.log(error);
         }
        },
        methods:{
            submitCreate: async function (){
                try{
                    let response = await ContactService.createContact(this.contact);
                    if(response){
                        return this.$router.push('/');
                    }else{
                        return this.$router.push('/contacts/add');
                    }
                }catch (error)
                {
                    console.log(error); 
                }
            }
        }
    }
    </script>
        
        
    <style scoped>
    </style>