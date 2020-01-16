<template>
    <div class="headerbox flexSpaceBetween">
         <div class="headerTitle flexTing">
            <img src="imgs/logo.png" />
            <div class="headerLetters">Task Manager</div>
        </div>
        <v-btn class="logoutBtn" v-on:click="logout()" >
            Logout
        </v-btn>
    </div>
</template>

<script>
export default {
    methods:{
        logout: function(){
            this.$router.push('/login');
        }
    }
}
</script>

<style scoped>
    @import url(./MainHeader.css);
</style>