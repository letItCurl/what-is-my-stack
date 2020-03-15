<template>
    <div class="Navbar">
        <nav class="deep-purple darken-1 hide-on-med-and-down">
            <div class="container">
                <router-link :to="{name: 'GMap'}" class="brand-logo left">What is my stack !</router-link>
                <ul class="right">
                    <li v-if="!user"><router-link :to="{name: 'Signup'}">Signup</router-link></li>
                    <li v-if="!user"><router-link :to="{name: 'Login'}">Login</router-link></li>
                    <li v-if="user"><a>{{user.email}}</a></li>
                    <li v-if="user"><a href="#" @click.prevent="logout">Logout</a></li>
                </ul>
            </div>
        </nav>
        <nav class="deep-purple darken-1 custom-navbar hide-on-large-only">
            <div class="container responsive-class">
                <div class="custom-brand-logo-div"><router-link :to="{name: 'GMap'}" class="custom-brand-logo center">What is my stack !</router-link></div>
                <div class="links-block">
                    <div class="link-item" v-if="!user"><router-link :to="{name: 'Signup'}">Signup</router-link></div>
                    <div class="link-item" v-if="!user"><router-link :to="{name: 'Login'}">Login</router-link></div>
                    <div class="link-item" v-if="user"><a>{{user.email}}</a></div>
                    <div class="link-item" v-if="user"><a href="#" @click.prevent="logout">Logout</a></div>
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
.custom-navbar{
    height: 85px;
}
.custom-brand-logo{
    font-size: 2em;
    
}
.link-item{
    width: 100%;
    height: 20px;
}

.links-block{
    display: flex;
    justify-content: center;
}
.custom-brand-logo-div{
    height: 40px;
}
</style>