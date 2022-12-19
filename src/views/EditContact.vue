<template>
    <div class="container mt-3">
     <div class="row">
         <div class="col">
             <p class="h3 text-success fw-bold">Edit Contact</p>
             <p class="fst-italic">Lorem ipsum dolor sit amet consectetur adipisicing elit. Voluptatum quos labore hic? Quisquam vero, illo odio commodi perspiciatis veniam suscipit iure temporibus, nobis natus vel cum recusandae. Debitis, provident deleniti?</p>
         </div>
     </div>
    </div>
 
    <div class="container mt-3">
         <div class="row">
             <div class="col-md-4">
                 <form @submit.prevent="updateSubmit()">
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
                         <select required class="form-control" v-model="contact.groupId" v-if="groups.length > 0">
                             <option value="">Select Group</option>
                             <option :value="group.id" v-for="group of groups" :key="group.id">{{group.name}}</option>
                         </select>
                     </div>
                     <div class="mb-2">
                         <input type="submit" class="btn btn-success" value="Update">
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
        name: 'EditContact',
        data: function(){
            return{
                contactId: this.$route.params.contactId,
                loading:false,
                contact: {
                    name: '',
                    photo:'',
                    email:'',
                    mobile:'',
                    company:'',
                    title:'',
                    groupId:''
                },
                errorMessage:null,
                groups:[]
            }
        },
        created: async function(){
            try{
                this.loading = true;
                let response = await ContactService.getContact(this.contactId);
                let groupResponse = await ContactService.getAllGroups();
                this.contact = response.data;
                this.groups = groupResponse.data;
                this.loading = false;
            }catch(error){
                this.errorMessage = error;
                this.loading = false;
            }
        },
        methods : {
            updateSubmit: async function() {
                try{
                    let response = await ContactService.updateContact(this.contact, this.contactId);
                    if(response){
                        return this.$router.push('/');
                    }else{
                        return this.$router.push(`/contacts/edit/${this.contactId}`);
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