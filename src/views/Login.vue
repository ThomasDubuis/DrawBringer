<template>
    <!-- <h1>Login form</h1>
    <form @submit.prevent="login">
        <input type="text" placeholder="Email" v-model="email"/><br>
        <input type="password" placeholder="password" v-model="password"/><br>
        <button>Sign Up</button>
    </form> -->
<div class="body">
    <div class="container" :class="{'right-panel-active':registerSide}" id="container">
        <div class="form-container sign-up-container">
            
            <form @submit.prevent="register">
                <h1>Créer un compte</h1>
                <div class="error">{{errorMessage}}</div>
                <input type="text" placeholder="Prénom" v-model="first_name" required/>
                <input type="text" placeholder="Nom" v-model="last_name" required/>
                <input type="email" placeholder="Email" v-model="email" required/>
                <input type="password" placeholder="Mot de passe" v-model="password" required/>
                <input type="password" placeholder="Confirmer le mot de passe" v-model="verifPassword" required/>
                <button class="signUp">S'enregistrer</button>
            </form>
        </div>
        <div class="form-container sign-in-container">
            <form @submit.prevent="login">
                <h1>Connexion</h1>
                <div class="error">{{errorMessage}}</div>
                <input type="email" placeholder="Email" v-model="email" required/>
                <input type="password" placeholder="Mot de passe" v-model="password" required/>
                <a href="#">Mot de passe oublié ?</a>
                <button class="signIn">Connexion</button>
            </form>
        </div>
        <div class="overlay-container">
            <div class="overlay">
                <div class="overlay-panel overlay-left">
                    <h1>Connexion</h1>
                    <p>Salut, si tu es déjà membre des DrawBringers connecte toi !</p>
                    <button class="ghost" id="signIn" v-on:click="changeSide()">Connexion</button>
                </div>
                <div class="overlay-panel overlay-right">
                    <h1>Créer un compte</h1>
                    <p>Viens rejoindre la communauté des DrawBringers</p>
                    <button class="ghost" id="signUp" v-on:click="changeSide()">Créer un compte</button>
                </div>
            </div>
        </div>
    </div>
</div>
</template>
<script>
import axios from 'axios'
export default {
    name: 'Login',
    data() {
        return {
            errorMessage:"",
            registerSide:false,

            email:'',
            password:'',
            verifPassword:'',
            first_name:'',
            last_name:''
        }
    },
    methods:{
        changeSide:function(){
            this.registerSide = !this.registerSide;
            this.errorMessage = "";
        },
        // async login(){
        //    const response = await axios.post('login', {
        //        email: this.email,
        //        password: this.password
        //    });
        //     localStorage.setItem('token',response.data.token);
        //     this.$router.push('/');
        // }
        login:async function(){
            var error="";
            await axios.post('api/users/login',{
                email: this.email,
                password: this.password
            }).then((res)=>{
                console.log(res.data);
                if (res.data.token && res.data.token !=null && res.data.token !="") {
                    localStorage.setItem('token',res.data.token);
                }else if(res.data.error && res.data.error !=null && res.data.error !=""){
                    error = res.data.error;
                }else{
                    error = "Une erreur inconnue est survenue"
                }
            }).catch((err)=>{
                console.log(err);
            })
            this.errorMessage = error;
        },
        register:async function(){
            if (this.password === this.verifPassword) {
                var error="";
                var registerSide = true;
                await axios.post('api/users/register',{
                    first_name: this.first_name,
                    last_name: this.last_name,
                    email: this.email,
                    password: this.password
                }).then((res)=>{
                    console.log(res.data);
                    if (res.data.userId && res.data.userId !=null && res.data.userId !="") {
                        registerSide = false;
                    }else if(res.data.error && res.data.error !=null && res.data.error !=""){
                        error = res.data.error;
                    }else{
                        error = "Une erreur inconnue est survenue"
                    }
                }).catch((err)=>{
                    console.log(err);
                })
            this.errorMessage = error;
            this.registerSide = registerSide;
            }else{
                this.errorMessage = "Mots de passe differents"
            }
            
        }

    }
}
</script>
<style scoped>
.body {
	margin:0;
	padding: 0;
	background: #f6f5f7;
	display: flex;
	justify-content: center;
	align-items: center;
	flex-direction: column;
	font-family: 'Montserrat', sans-serif;
	height: 100vh;
}

h1 {
	font-weight: bold;
	margin: 0;
}

h2 {
	text-align: center;
}

p {
	font-size: 14px;
	font-weight: 100;
	line-height: 20px;
	letter-spacing: 0.5px;
	margin: 20px 0 30px;
}

span {
	font-size: 12px;
}

a {
	color: #333;
	font-size: 14px;
	text-decoration: none;
	margin: 15px 0;
}

a:hover{
	color: #9b59b6;
}

button {
	border-radius: 20px;
	border: 1px solid #9b59b6;
	/* background-color: #2ecc71; */
	color: #FFFFFF;
	font-size: 12px;
	font-weight: bold;
	padding: 12px 45px;
	letter-spacing: 1px;
	text-transform: uppercase; /*Transforme en Majuscule*/
	transition: transform 80ms ease-in;
}
button.signUp {
	border: 1px solid #2ecc71;
	background: -webkit-linear-gradient(to right, #2ecc71, #27ae60, #27ae60);
	background: linear-gradient(to right, #2ecc71, #27ae60);
}
button.signIn {
	background: -webkit-linear-gradient(to right, #9b59b6, #8e44ad);
	background: linear-gradient(to right, #9b59b6, #8e44ad);
}
button:hover {
	cursor: pointer;
}
button:active {
	transform: scale(0.95);
}

button:focus {
	outline: none;
}

button.ghost {
	background: transparent;
	border-color: #FFFFFF;
}

form {
	background-color: #FFFFFF;
	display: flex;
	align-items: center;
	justify-content: center;
	flex-direction: column;
	padding: 0 45px;
	height: 100%;
	text-align: center;
}

input {
	background-color: #eee;
	border: none;
	padding: 12px 15px;
	margin: 8px 0;
	width: 100%;
}

.error {
	height: 15px;
	color: #e74c3c;
	font-size: 12px;
}

.container {
	background-color: #fff;
	border-radius: 10px;
    box-shadow: 0 14px 28px rgba(0,0,0,0.25), 0 10px 10px rgba(0,0,0,0.22);
	position: relative;
	overflow: hidden;
	width: 768px;
	max-width: 100%;
	min-height: 480px;
}

.form-container {
	position: absolute;
	top: 0;
	height: 100%;
	transition: all 0.6s ease-in-out;
}

.sign-in-container {
	left: 0;
	width: 50%;
	z-index: 2;
}

.container.right-panel-active .sign-in-container {
	transform: translateX(100%);
}

.sign-up-container {
	left: 0;
	width: 50%;
	opacity: 0;
	z-index: 1;
}

.container.right-panel-active .sign-up-container {
	transform: translateX(100%);
	opacity: 1;
	z-index: 5;
	animation: show 0.6s;
}

@keyframes show {
	0%, 49.99% {
		opacity: 0;
		z-index: 1;
	}
	
	50%, 100% {
		opacity: 1;
		z-index: 5;
	}
}

.overlay-container {
	position: absolute;
	top: 0;
	left: 50%;
	width: 50%;
	height: 100%;
	overflow: hidden;
	transition: transform 0.6s ease-in-out;
	z-index: 100;
}

.container.right-panel-active .overlay-container{
	transform: translateX(-100%);
}

.overlay {
	background: #2ecc71;
	background: -webkit-linear-gradient(to right, #2ecc71, #9b59b6);
	background: linear-gradient(to right, #2ecc71, #9b59b6);
	background-repeat: no-repeat;
	background-size: cover;
	background-position: 0 0;
	color: #FFFFFF;
	position: relative;
	left: -100%;
	height: 100%;
	width: 200%;
    transform: translateX(0);
	transition: transform 0.6s ease-in-out;
}

.container.right-panel-active .overlay {
    transform: translateX(50%);
}

.overlay-panel {
	position: absolute;
	display: flex;
	align-items: center;
	justify-content: center;
	flex-direction: column;
	padding: 0 40px;
	text-align: center;
	top: 0;
	height: 100%;
	width: 50%;
	transform: translateX(0);
	transition: transform 0.6s ease-in-out;
}

.overlay-left {
	transform: translateX(-20%);
}

.container.right-panel-active .overlay-left {
	transform: translateX(0);
}

.overlay-right {
	right: 0;
	transform: translateX(0);
}

.container.right-panel-active .overlay-right {
	transform: translateX(20%);
}
</style>