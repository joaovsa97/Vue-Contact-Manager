<template>
    <div class="container mt-3">
        <div class="row">
            <div class="col">
                <p class="h3 text-success fw-bold">Edit Contact</p>
                <p class="fst-italic">Lorem, ipsum dolor sit amet consectetur adipisicing elit. Fugit saepe assumenda magnam ipsum atque rerum esse sapiente aspernatur optio qui ipsa ullam vero odio recusandae possimus unde explicabo, dolore iste.</p>
            </div>
        </div>
    </div>

    <div v-if="loading">
        <div class="container">
            <div class="row">
                <div class="col">
                    <Spinner />
                </div>
            </div>
        </div>
    </div>
    
    <div v-if="!loading && errorMessage">
        <div class="container mt-3">
            <div class="row">
                <div class="col">
                    <p class="h4 tw-bold text-danger">{{errorMessage}}</p>
                </div>
            </div>
        </div>
    </div>

    <div class="container mt-3">
        <div class="row">
            <div class="col-md-4">
                <form @submit.prevent="updateSubmit()">
                    <div class="mb-2">
                        <input v-model="contact.name" type="text" class="form-control" placeholder="Name">
                    </div>
                    <div class="mb-2">
                        <input v-model="contact.photo" type="text" class="form-control" placeholder="Photo URL">
                    </div>
                    <div class="mb-2">
                        <input v-model="contact.email" type="email" class="form-control" placeholder="E-mail">
                    </div>
                    <div class="mb-2">
                        <input v-model="contact.Mobile" type="number" class="form-control" placeholder="Mobile">
                    </div>
                    <div class="mb-2">
                        <input v-model="contact.company" type="text" class="form-control" placeholder="Company">
                    </div>
                    <div class="mb-2">
                        <input v-model="contact.title" type="text" class="form-control" placeholder="Title">
                    </div>
                    <div class="mb-b">
                        <input type="submit" class="btn btn-success" value="Update">
                    </div>
                </form>
            </div>
            <div class="col-md-4">
                <img :src="contact.photo" alt="" class="contact-image">
            </div>
        </div>

    </div>
</template>

<script>
import Spinner from "@/components/Spinner.vue"
import { ContactServices } from '@/services/ContactServices'
    export default {
        name:'EditContact',
        components : {Spinner},
        data : function() {
            return{
                contactId : this.$route.params.contactId,
                loading : false,
                contact : {
                    name: '',
                    photo: '',
                    email: '',
                    mobile: '',
                    company: '',
                    title: ''
                },
                errorMessage : null
            }
        },
        created : async function (){
            try {
                this.loading = true;
                let response = await ContactServices.getContact(this.contactId);
                this.contact = response.data;
                this.loading = false;
            } catch (error) {
                this.errorMessage = error;
                this.loading = false;
            }
        },
        methods : {
            updateSubmit : async function (){
                try{
                    let response = await ContactServices.updateContact(this.contact, this.contactId);
                    if(response){
                        return this.$router.push('/');
                    }
                    else{
                        return this.$router.push(`/contacts/view/${this.contactId}`);
                    }
                }
                catch(error){
                    console.log(error);
                }
            }
        },
    }
</script>

<style scoped>

</style>