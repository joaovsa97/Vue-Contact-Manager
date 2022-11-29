<template>
    <div class="container mt-3">
        <div class="row">
            <div class="col">
                <p class="h3 text-success fw-bold">View Contact</p>
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

    <div class="container" v-if="!loading && isDone()">
        <div class="row align-items-center">
            <div class="col-md-4">
                <img :src="contact.photo" alt="" class="contact-image-big">
            </div>
            <div class="col-md-6">
                <ul class="list-group">
                    <li class="list-group-item">Name: <span class="fw-bold">{{contact.name}}</span></li>
                    <li class="list-group-item">E-mail: <span class="fw-bold">{{contact.email}}</span></li>
                    <li class="list-group-item">Mobile Number: <span class="fw-bold">{{contact.Mobile}}</span></li>
                    <li class="list-group-item">Company: <span class="fw-bold">{{contact.company}}</span></li>
                    <li class="list-group-item">Title: <span class="fw-bold">{{contact.title}}</span></li>
                </ul>
            </div>
        </div>
        <div class="row mt-3">
            <div class="col">
                <router-link to="/" class="btn btn-success">
                    <i class="fa fa-arrow-alt-circle-left"></i> Back
                </router-link>
            </div>
        </div>
    </div>
</template>

<script>
import { ContactServices } from '@/services/ContactServices'
import Spinner from "@/components/Spinner.vue"

    export default {
        name:'ViewContact',
        components:{Spinner},
        data : function (){
            return {
                contactId : this.$route.params.contactId,
                loading : false,
                contact : {},
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
            isDone : function (){
                return Object.keys(this.contact).length > 0
            }
        }
    }
</script>

<style scoped>

</style>