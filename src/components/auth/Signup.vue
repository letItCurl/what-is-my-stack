<template>
    <div class='signup container'>
        <form @submit.prevent="signup" class="card-panel">
            <h2 class="center deep-purple-text">Signup</h2>
            <div class="field">
                <label for="email">Email: </label>
                <input type="email" v-model="email" autocomplete="off">
            </div>
            <div class="field">
                <label for="password">Password: </label>
                <input type="password" v-model="password" autocomplete="off">
            </div>
            <div class="field">
                <label for="alias">Alias: </label>
                <input type="text" v-model="alias" autocomplete="off">
            </div>
            <p class="red-text center" v-if="feedback">{{feedback}}</p>
            <div class="field center">
                <button class="btn deep-purple">
                    Signup
                </button>
            </div>
        </form>
    </div>
</template>

<script>
import slugify from 'slugify'
import db from '@/firebase/init'
import firebase, { auth } from 'firebase'

export default {
    name: 'Signup',
    data(){
        return{
            email: null,
            password: null,
            alias: null,
            feedback: null,
            slug: null
        }
    },
    methods: {
        signup(){
            if(this.alias && this.email && this.password){
                this.slug = slugify(this.alias, {
                    replacement: '-',
                    remove: /[$*_+~.()'"!\-:@]/g,
                    lower: true
                })
                let ref = db.collection('users').doc(this.slug)
                ref.get()
                .then(doc => {
                    if(doc.exists){
                        this.feedback = 'this alias already exists'
                    }
                    else{
                        firebase.auth().createUserWithEmailAndPassword(this.email, this.password)
                        .then(cred => {
                            ref.set({
                                alias: this.alias,
                                geolocation: null,
                                user_id: cred.user.uid
                            }).then(() => this.$router.push({name: 'GMap'}))
                        })
                        .catch(e => {console.log(e); this.feedback = e.message})
                        this.feedback = 'Loging...'
                    }})
            }else{
                this.feedback = "you must fill all fields"
            }
        }
    }
    
}
</script>

<style>
.signup{
    max-width: 400px;
    margin-top: 60px;
}
.signup h2{
    font-size: 2.4em;
}
.signup .field{
    margin-bottom: 16px;
}
</style>