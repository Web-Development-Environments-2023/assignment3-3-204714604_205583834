<template>
  <div id="app">
    <div id="nav">
      <b-navbar  class="fullBar" variant="success">
            <b-navbar>
                <router-link :to="{ name: 'main' }">Vue Recipes</router-link>|
                <router-link :to="{ name: 'search' }">Search</router-link>|
                <router-link :to="{ name: 'about' }">About</router-link>
            </b-navbar>

            <b-navbar-nav class="ml-auto">
              <span class="navbar-text space" v-if="!$root.store.username"> <b>Hello Guest</b> </span>
              <span class="navbar-text space" v-if="$root.store.username"> <b>Hello {{$root.store.username}}</b> </span>
              <b-nav-item-dropdown :text="$root.store.username" v-if="$root.store.username" right>
                  Personal:
                  <router-link tag="" :to="{ name: 'main' }" @click.native="$root.store.logout">Logout</router-link>
                  <router-link tag="b-dropdown-item" :to="{ name: 'favorites' }">favorites</router-link>
                  <router-link tag="b-dropdown-item" :to="{ name: 'MyRecipes' }">My Recipes</router-link>
                  <router-link tag="b-dropdown-item" :to="{ name: 'myFamilyRecipes' }">My Family Recipes</router-link>
                  <b-button id="show-modal" @click="showModal = true">New Recipe</b-button>
                  <Teleport to="body">
                    <modal :show="showModal" @close="showModal = false">
                      <template #header>
                        <h3>New Recipe</h3>
                      </template>
                    </modal>
                </Teleport>
              </b-nav-item-dropdown>
              <b-button variant="danger" @click="Logout" v-if="$root.store.username">Logout</b-button>
              <b-button :to="{ name: 'register' }" variant="danger" @click="register" v-if="!$root.store.username" class="space">Register</b-button>
              <b-button :to="{ name: 'login' }" variant="danger" @click="login" v-if="!$root.store.username">Login</b-button>   





            </b-navbar-nav>
        </b-navbar>
    </div>
    <router-view />
  </div>
</template>

<script>
import Modal from './components/Modal.vue'
export default {
  components: {
    Modal
  },
  data() {
    return {
      showModal: false
    }
  },
  name: "App",
  methods: {
    Logout() {
      this.$root.store.logout();
      this.$root.toast("Logout", "User logged out successfully", "success");

      this.$router.push("/").catch(() => {
        this.$forceUpdate();
      });
    }
  }
};
</script>

<style lang="scss">
@import "@/scss/form-style.scss";

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
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
  color: #2c3e50;
}

#show-modal {
  font-weight: bold;
  color: #2c3e50;
  background-color: white;
  border: 0px;
}

.space{
  margin-right: 10px;
}
</style>
