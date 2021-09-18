<template>
  <header>
    <nav class="nav-container">
      <div class="page-title">
        <router-link class="header-title" :to="{ name: 'Home' }"
          >My Book Collection</router-link
        >
      </div>
      <div class="nav-links" v-show="!isMobile">
        <ul>
          <router-link class="link" :to="{ name: 'Home' }"
            >Discover Books</router-link
          >
          <router-link class="link" :to="{ name: 'About' }"
            >My Books</router-link
          >
          <router-link class="link" :to="{ name: 'About' }"
            >Login/Register</router-link
          >
        </ul>
      </div>
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
        <router-link class="link" @click="toggleSidebar" :to="{ name: 'Home' }"
          >Discover Books</router-link
        >
        <router-link class="link" @click="toggleSidebar" :to="{ name: 'About' }"
          >My Books</router-link
        >
        <router-link class="link" @click="toggleSidebar" :to="{ name: 'About' }"
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
    const isMobile = ref(null);
    const showSidebar = ref(false);
    const windowWidth = ref(null);

    // figure out if the screen is small enough to be a mobile device
    const checkScreen = () => {
      windowWidth.value = window.innerWidth;
      if (windowWidth.value <= 800) {
        isMobile.value = true;
      } else {
        isMobile.value = false;
      }
      return;
    };

    // flip the sidebar
    const toggleSidebar = () => {
      showSidebar.value = !showSidebar.value;
    };

    checkScreen();
    // check on window resize if it's small enough for mobile view
    document.addEventListener.call(window, "resize", () => {
      checkScreen();
    });

    return { isMobile, showSidebar, toggleSidebar };
  },
};
</script>

<style scoped>
header {
  background-color: #eee;
  padding: 12px 25px;
}

.nav-container {
  display: flex;
}

.page-title {
  display: flex;
  align-items: center;
}

.header-title {
  text-decoration: none;
  font-size: 24px;
  font-weight: bold;
  color: black;
}

.nav-links {
  position: relative;
  display: flex;
  align-items: center;
  justify-content: flex-end;
  flex: 1;
}

.mobile-nav {
  background-color: black;
  position: fixed;
  width: 70%;
  max-width: 250px;
  height: 100%;
  top: 0;
  left: 0;
  padding-top: 20px;
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
  padding: 15px 50px;
}

.nav-links .link {
  padding-right: 40px;
}

.nav-links .link:last-child {
  margin-right: 30px;
  padding-right: 0;
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
</style>
