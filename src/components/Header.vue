<template>
  <header>
    <nav class="nav-links" v-show="!isMobile">
      <ul>
        <router-link class="link" :to="{ name: 'Home' }">Home</router-link>
        <router-link class="link" :to="{ name: 'About' }">About</router-link>
        <router-link class="link" :to="{ name: 'About' }"
          >Login/Register</router-link
        >
      </ul>
    </nav>

    <button
      @click="toggleSidebar"
      class="navbar-toggle"
      type="button"
      data-toggle="collapse"
      data-target=".bs-navbar-collapse"
      v-show="isMobile"
    >
      <span class="icon-bar"></span>
      <span class="icon-bar"></span>
      <span class="icon-bar"></span>
    </button>

    <transition name="mobile-nav">
      <ul class="mobile-nav" v-show="isMobile && showSidebar">
        <router-link class="link" :to="{ name: 'Home' }">Home</router-link>
        <router-link class="link" :to="{ name: 'About' }">About</router-link>
        <router-link class="link" :to="{ name: 'About' }"
          >Login/Register</router-link
        >
      </ul>
    </transition>
  </header>
</template>

<script>
import { ref } from "vue";
export default {
  name: "Header",
  setup() {
    const isMobile = ref(false);
    const showSidebar = ref(false);
    const windowWidth = ref(null);

    function checkScreen() {
      windowWidth.value = window.innerWidth;
      if (windowWidth.value <= 750) {
        isMobile.value = true;
        return;
      }
      isMobile.value = false;
      return;
    }

    function toggleSidebar() {
      showSidebar.value = !showSidebar.value;
    }

    checkScreen();
    document.addEventListener.call(window, "resize", () => {
      checkScreen();
    });

    return { isMobile, checkScreen, showSidebar, toggleSidebar };
  },
};
</script>

<style scoped>
header {
  background-color: #eee;
  /* padding: 5px 0; */
  font-family: Verdana, Tahoma, sans-serif;
}

.nav-links {
  position: relative;
  display: flex;
  justify-content: flex-end;
}

.mobile-nav {
  background-color: black;
  position: fixed;
  width: 70%;
  max-width: 250px;
  height: 100%;
  top: 0;
  left: 0;
  margin-top: 0;
}

.mobile-nav-enter-active,
.mobile-nav-leave-active {
  transition: all 1s ease;
}
.mobile-nav-enter-from {
  transform: translateX(-300px);
}
.mobile-nav-enter-to {
  transform: translateX(0);
}
.mobile-nav-leave-to {
  transform: translateX(-300px);
}

.mobile-nav .link {
  display: flex;
  flex-direction: column;
  padding: 15px 0;
}

.nav-links .link {
  padding-right: 20px;
}

.nav-links .link:last-child {
  margin-right: 30px;
}

.link {
  text-decoration: none;
  font-weight: bold;
  transition: 1s color ease;
}

.nav-links .link,
.nav-links .link:visited {
  color: black;
}

.mobile-nav .link,
.mobile-nav .link:visited {
  color: white;
}

.link:hover {
  color: brown;
}

.navbar-toggle {
  position: absolute;
  top: 18px;
  right: 25px;
  border: 1px solid transparent;
  border-radius: 4px;
  cursor: pointer;
}

.navbar-toggle .icon-bar {
  display: block;
  width: 22px;
  height: 2px;
  background-color: #000000;
  border-radius: 1px;
}

.navbar-toggle .icon-bar + .icon-bar {
  margin-top: 4px;
}

@media screen and (max-width: 750px) {
  header {
    padding: 25px 0;
  }
}
</style>
