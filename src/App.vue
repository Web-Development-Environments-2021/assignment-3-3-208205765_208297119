<template>
  <div id="app">
    <b-navbar toggleable="lg" type="dark" variant="info">
      <b-navbar-brand v-if="!$root.store.username">Hello Guest</b-navbar-brand>
      <b-navbar-brand v-else>Hello {{$root.store.username}}</b-navbar-brand>
      <b-navbar-nav>
         <b-nav-item :to="{ name: 'main' }">Main Page</b-nav-item>
         <b-nav-item :to="{name: 'about'}">About</b-nav-item>
         <b-nav-item :to="{name: 'currentStageGames'}">Current Stage Games</b-nav-item>
          <b-nav-item :to="{ name: 'search' }">Search</b-nav-item>
      </b-navbar-nav>
      <b-collapse id="nav-collapse" is-nav>
             
        <b-navbar-nav class="ml-auto" v-if="!$root.store.username">
          <b-nav-item :to="{ name: 'login' }">Login</b-nav-item>
          <b-nav-item :to="{ name: 'register' }">Register</b-nav-item>
        </b-navbar-nav>
        <b-navbar-nav class="ml-auto" v-else>
        <b-nav-item-dropdown right>
          <template #button-content>
            {{$root.store.username}}
          </template>
          <b-dropdown-item href="#" v-if="$root.store.isAdmin" @click="moveToAddGamesToSystemPage">Add GAmes To System</b-dropdown-item>
          <b-dropdown-item href="#" @click="moveToFavoriteGames">My Games</b-dropdown-item>
          <b-dropdown-item href="#" @click="Logout">Log Out</b-dropdown-item>
        </b-nav-item-dropdown>
        </b-navbar-nav>
      </b-collapse>
    </b-navbar>
    <router-view />
  </div>
</template>

<script>
export default {
  name: "App",
  data(){
    return{
      isAdmin:false
    }
  },
  
  methods: {
    Logout() {
      this.$root.store.logout();
      this.$root.toast("Logout", "User logged out successfully", "success");
      this.isAdmin=false;
      this.$router.push("/").catch(() => {
        this.$forceUpdate();
      });
    },
    moveToFavoriteGames(){
      this.$router.push({name:"favoriteGamesPage"});
    },
    moveToAddGamesToSystemPage(){
      this.$router.push({name:"AddGamesToSystemPage"});
    }
  
  }
};
</script>

<style lang="scss">
@import "@/scss/form-style.scss";

#app {
  // font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  min-height: 100vh;
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
