<template>
  <div id="app">
   <router-view />
  </div>
</template>

<script>
export default {
  name:"app",

  beforeCreate()
     {
      if (!localStorage.isUserloggedin) {
            this.$router.push('/');
        }  
     }
}
</script>
<style>
@import "https://cdnjs.cloudflare.com/ajax/libs/font-awesome/5.10.0-11/css/all.min.css";
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  height: 100%;
}

#nav {
  padding: 30px;
}

#nav a {
  font-weight: bold;
  color: #2c3e50;
}

#nav a.router-link-exact-active {
  color: #42b983;
}
</style>
