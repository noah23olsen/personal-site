<template>
  <div class="container-fluid" v-if="currentTitle !== 'about'">
    <div class="row text-center">
      <div class="col-12 bg-cyan">
        <title-vue :titleText="currentTitle" />
      </div>
    </div>
    <div class="row d-flex flex-row bg-cyan justify-content-center text-center py-3">
      <div class="col-4"> <router-link to="/">home</router-link></div>
      <div class="col-4"> <router-link to="/music">listens</router-link></div>
      <div class="col-4"> <router-link to="/readings">readings</router-link></div>
    </div>
    <div class="row bg-cyan px-5">
      <div class="col-12 bg-white p-5">
        <router-view></router-view>
      </div>
    </div>
    <div class="row text-center bg-cyan d-flex justify-content-center align-items-center">
      <div class="col-12 py-3">
        <p>inspired by <a href="https://ranprieur.com/">ran prieur</a></p>
      </div>
    </div>
  </div>

  <div v-else class="page-container">
    <div class="content-wrapper">
      <a class="text-white" href="https://www.google.com/search?q=the+answer+to+life%2C+the+universe%2C+and+everything"
        target="_blank">42
      </a>
    </div>

    <div class="social-links">
      <div class="d-flex w-50 justify-content-between mx-auto">
        <a class="links-color" href="https://github.com/noah23olsen" target="_blank">github</a>
        <a class="links-color" href="https://twitter.com/noaholsen_" target="_blank">x</a>
        <a class="links-color" href="https://www.linkedin.com/in/noaholsen-/" target="_blank">linkedin</a>
      </div>
    </div>
  </div>

  <div class="sparkle-container"></div>
</template>

<script>
import TitleVue from "./components/TitleVue.vue";

export default {
  name: "App",
  components: {
    TitleVue,
  },
  data() {
    return {
      currentTitle: "",
    };
  },
  watch: {
    $route(to) {
      this.currentTitle = to.meta.title || "-";
    },
  },
  mounted() {
    document.addEventListener('mousemove', this.createSparkle);
  },
  beforeUnmount() {
    document.removeEventListener('mousemove', this.createSparkle);
  },
  methods: {
    createSparkle(e) {
      const sparkle = document.createElement('div');
      sparkle.className = 'sparkle';
      sparkle.style.left = e.clientX + 'px';
      sparkle.style.top = e.clientY + 'px';

      // Random size between 2 and 6 pixels
      const size = Math.random() * 4 + 2;
      sparkle.style.width = size + 'px';
      sparkle.style.height = size + 'px';

      // Random rotation
      sparkle.style.transform = `rotate(${Math.random() * 360}deg)`;

      document.querySelector('.sparkle-container').appendChild(sparkle);

      // Remove sparkle after animation
      setTimeout(() => {
        sparkle.remove();
      }, 1000);
    }
  }
};
</script>

<style>
.links-color {
  color: rgba(255, 255, 255, 0.746);
}

.bg-cyan {
  background-color: rgb(149, 188, 192);
}

html,
body {
  margin: 0;
  padding: 0;
  overflow: hidden;
  background-color: black;
  height: 100%;
}

#app {
  height: 100%;
  background-color: black;
}

.sparkle-container {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  pointer-events: none;
  z-index: 9999;
}

.sparkle {
  position: absolute;
  background: linear-gradient(45deg, #fff, rgba(255, 255, 255, 0.8));
  border-radius: 50%;
  animation: sparkle-fade 1s ease-out forwards;
}

@keyframes sparkle-fade {
  0% {
    opacity: 1;
    transform: scale(1) rotate(0deg);
  }

  100% {
    opacity: 0;
    transform: scale(0) rotate(360deg);
  }
}

.page-container {
  display: flex;
  flex-direction: column;
  min-height: 100vh;
  background-color: black;
}

.content-wrapper {
  flex: 1;
  display: flex;
  justify-content: center;
  align-items: center;
  padding: 20px;
}

.social-links {
  padding-bottom: 15px;
  background-color: black;
  position: relative;
  z-index: 1;
}

@media (max-width: 768px) {
  .page-container {
    min-height: -webkit-fill-available;
  }

  .social-links {
    padding-bottom: calc(10px + env(safe-area-inset-bottom));
  }
}
</style>