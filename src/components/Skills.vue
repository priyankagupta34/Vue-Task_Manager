<template>
  <div class="hello">

    <button v-on:click="changeName" v-bind:disabled="btnState" >Change Name</button>
    <ul>
      <li v-for="(data, index) in skills" :key="index">
        {{index}}. {{data.skills}}
      </li>
    </ul>

    <div v-bind:class="{ alert: showAlert }"></div>
  </div>
</template>

<script>
export default {
  data() {
    return{
      skills: [
        {"skills": "vue"},
        {"skills": "front"},
        {"skills": "test2"},
        {"skills": "test3"},
      ],
      showAlert: true
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style src="./Skills.css" scoped>

</style>
