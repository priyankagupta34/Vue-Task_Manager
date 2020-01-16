<template>
    <div class="headerBox ">
        <div class="flexSpaceBetween">
            <div>
                <div class="headerTitle flex">
                    <img src="imgs/logo.png" />
                    <div class="headerLetters">Task Manager</div>
                </div>
            </div>
            <div>
                <v-btn class="signbtn" v-on:click="toggleSignupToLoginBtn()" v-if="!showLoginBtn">
                    Signup
                </v-btn>
                <v-btn class="signbtn" v-on:click="toggleSignupToLoginBtn()" v-if="showLoginBtn">
                    Login
                </v-btn>
            </div>
        </div>
        <div class="mainpara">
            <div>Managing task at one place is the new trends. Adopting the best practice is always the first challenge.  </div>
            <div>Be a part of world's largest task management forum!</div>
        </div>
    </div>
</template>

<script>
export default {
    data(){
        return{
            showLoginBtn:false
        }
    },
    methods: {
        toggleSignupToLoginBtn: function(){
            if(this.showLoginBtn){
                this.showLoginBtn = false;
            this.$router.push('/signup').catch((err)=>{
                console.log('err going to signup ', err);
            })
            }else{
                this.showLoginBtn = true;
                this.$router.push('/login').catch((err)=>{
                console.log('err going to login ', err);})
            }
        }
    },
    created: function() {
        window.console.log('here', this.showLoginBtn);
    }
}
</script>

<style src="./Header.css" scoped>

</style>