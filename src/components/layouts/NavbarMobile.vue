<template>
    <div class="navbar">
        <nav class="deep-purple darken-1">
            <div class="container responsive-class">
                <div><router-link :to="{name: 'GMap'}" class="custom-brand-logo center">What is my stack !</router-link></div>
                <div class="links-block">
                    <div v-if="!user"><router-link :to="{name: 'Signup'}">Signup</router-link></div>
                    <div v-if="!user"><router-link :to="{name: 'Login'}">Login</router-link></div>
                    <div v-if="user"><a>{{user.email}}</a></div>
                    <div v-if="user"><a href="#" @click.prevent="logout">Logout</a></div>
                </div>
            </div>
        </nav>
    </div>
</template>

<script>
import firebase from 'firebase'
export default {
    name: 'Navbar',
    data(){
        return{
            user: null
        }
    },
    methods: {
        logout(){
            firebase.auth().signOut().then(() => {
                this.$router.push({name: 'Login'})
            })
        }
    },
    created(){
        //let user = firebase.auth().currentUser
        firebase.auth().onAuthStateChanged((user) =>{
            if(user){
                this.user = user
            }else{
                this.user = null
            }
        })
    }
}
</script>
<style>
.responsive-class{
    display: inline;
    text-align: center;
}
.navbar nav{
    height: 180px;
}
.custom-brand-logo{
    font-size: 2em;
}
</style>