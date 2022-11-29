<template>
    <div class="container mt-3">
        <div class="row">
            <div class="col">
                <p class="h3 text-success fw-bold">Add Contact</p>
                <p class="fst-italic">Lorem, ipsum dolor sit amet consectetur adipisicing elit. Fugit saepe assumenda magnam ipsum atque rerum esse sapiente aspernatur optio qui ipsa ullam vero odio recusandae possimus unde explicabo, dolore iste.</p>
            </div>
        </div>
    </div>
    <div class="container mt-3">
        <div class="row">
            <div class="col-md-4">
                <form @submit.prevent="submitCreate">
                    <div class="mb-2">
                        <input required v-model="contact.name" type="text" class="form-control" placeholder="Name">
                    </div>
                    <div class="mb-2">
                        <input v-model="contact.photo" type="text" class="form-control" placeholder="Photo URL">
                    </div>
                    <div class="mb-2">
                        <input required v-model="contact.email" type="email" class="form-control" placeholder="E-mail">
                    </div>
                    <div class="mb-2">
                        <input required v-model="contact.mobile" type="number" class="form-control" placeholder="Mobile">
                    </div>
                    <div class="mb-2">
                        <input required v-model="contact.company" type="text" class="form-control" placeholder="Company">
                    </div>
                    <div class="mb-2">
                        <input required v-model="contact.title" type="text" class="form-control" placeholder="Title">
                    </div>
                    <div class="mb-b">
                        <input type="submit" class="btn btn-success" value="Create">
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
import { ContactServices } from '@/services/ContactServices';

    export default {
        name:'AddContact',
        data : function () {
            return{
                contact : {
                    name: '',
                    photo: '',
                    email: '',
                    mobile: '',
                    company: '',
                    title: ''
                }
            }
        },

        methods : {
            submitCreate : async function (){
                try{
                    let response = await ContactServices.createContact(this.contact);
                    if(response){
                        return this.$router.push('/');
                    }
                    else{
                        return this.$router.push('/contacts/add');
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