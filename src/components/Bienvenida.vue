<template>
  <div>
    <!-- Hero -->
    <div class="relative h-screen overflow-hidden">
      <div
        ref="parallaxBackground"
        class="absolute top-0 left-0 w-full h-full bg-cover bg-center"
        :style="{ backgroundImage: `url(${heroImage})` }"
      ></div>
      <div class="absolute inset-0 bg-black opacity-30"></div>

      <!-- Lens Flare Animation -->
      <div class="absolute inset-0 overflow-hidden pointer-events-none">
        <div class="lens-flare lens-flare-1"></div>
        <div class="lens-flare lens-flare-2"></div>
        <div class="lens-flare lens-flare-3"></div>
        <div class="lens-flare lens-flare-4"></div>
      </div>

      <div class="relative h-full flex items-center justify-center z-10">
        <div class="text-center text-white p-5">
          <img
            src="https://i.ibb.co/Vv8tSFb/logo-sin-texto.png"
            alt="Logo de la Iglesia Bautista Gracia en Abundancia"
            class="mx-auto mb-6 w-24 h-24 sm:w-32 sm:h-32"
          />
          <div class="font-semibold">
            <p class="text-xl sm:text-2xl md:text-3xl mb-1">Bienvenido a</p>
            <h1 class="leading-none">
              <span
                class="block text-6xl sm:text-7xl md:text-8xl font-colus tracking-wide sm:tracking-normal text-white"
                >GRACIA</span
              >
              <span
                class="block text-3xl sm:text-4xl md:text-5xl sm:-mt-1 md:-mt-2 font-colus"
                >EN ABUNDANCIA</span
              >
              <span
                class="block text-xl sm:text-2xl md:text-3xl mt-0 sm:mt-1 md:mt-2"
                >Iglesia Bautista</span
              >
            </h1>
          </div>
        </div>
      </div>
      <!-- Scroll/Swipe Indicator -->
      <div class="absolute bottom-20 left-1/2 transform -translate-x-1/2 z-20">
        <div class="scroll-indicator">
          <!-- Indicador para móviles (deslizar) -->
          <div class="sm:hidden flex flex-col items-center">
            <span class="text-white text-sm mb-2">Deslizar</span>
            <svg
              class="w-6 h-6 text-white"
              fill="none"
              stroke="currentColor"
              viewBox="0 0 24 24"
              xmlns="http://www.w3.org/2000/svg"
            >
              <path
                stroke-linecap="round"
                stroke-linejoin="round"
                stroke-width="2"
                d="M5 10l7-7m0 0l7 7m-7-7v18"
              ></path>
            </svg>
          </div>
          <!-- Indicador para pantallas más grandes (bajar) -->
          <div class="hidden sm:flex flex-col items-center">
            <span class="text-white text-sm mb-2">Bajar</span>
            <div class="w-6 h-10 border-2 border-white rounded-full p-1">
              <div
                class="w-1 h-3 bg-white rounded-full animate-bounce mx-auto"
              ></div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      heroImage: "https://i.ibb.co/f87nwPw/biblia2.jpg",
    };
  },
  mounted() {
    window.addEventListener("scroll", this.handleScroll);
  },
  beforeUnmount() {
    window.removeEventListener("scroll", this.handleScroll);
  },
  methods: {
    handleScroll() {
      const parallaxBackground = this.$refs.parallaxBackground;
      if (parallaxBackground) {
        const scrollPosition = window.pageYOffset;
        const limit = parallaxBackground.offsetHeight;
        if (scrollPosition <= limit) {
          parallaxBackground.style.transform = `translateY(${
            scrollPosition * 0.5
          }px)`;
        }
      }
    },
  },
};
</script>

<style scoped>
.overflow-hidden {
  overflow: hidden;
}
.scroll-indicator {
  display: flex;
  flex-direction: column;
  align-items: center;
  animation: fadeInOut 2s infinite;
}
@keyframes fadeInOut {
  0%,
  100% {
    opacity: 0.5;
  }
  50% {
    opacity: 1;
  }
}

/* Animación deslizar */
@keyframes swipeAnimation {
  0%,
  100% {
    transform: translateY(0);
  }
  50% {
    transform: translateY(-10px);
  }
}
.sm\:hidden svg {
  animation: swipeAnimation 2s infinite;
}

/* Lens Flare Animation */
.lens-flare {
  position: absolute;
  width: 200px;
  height: 200px;
  filter: blur(20px);
  opacity: 0.7;
  mix-blend-mode: screen;
}

.lens-flare-1 {
  background: radial-gradient(circle, #001e62 0%, rgba(0, 30, 98, 0) 70%);
  animation: lensFlareAnimation1 20s ease-in-out infinite;
}

.lens-flare-2 {
  background: radial-gradient(circle, #6e2b62 0%, rgba(110, 43, 98, 0) 70%);
  animation: lensFlareAnimation2 20s ease-in-out infinite;
}

.lens-flare-3 {
  background: radial-gradient(circle, #a50034 0%, rgba(165, 0, 52, 0) 70%);
  animation: lensFlareAnimation3 20s ease-in-out infinite;
}

@keyframes lensFlareAnimation1 {
  0%,
  100% {
    transform: translate(-50%, -50%) scale(0.8);
    top: 25%;
    left: 25%;
  }
  25% {
    transform: translate(-50%, -50%) scale(1.2);
    top: 75%;
    left: 75%;
  }
  50% {
    transform: translate(-50%, -50%) scale(1);
    top: 50%;
    left: 50%;
  }
  75% {
    transform: translate(-50%, -50%) scale(1.5);
    top: 25%;
    left: 75%;
  }
}

@keyframes lensFlareAnimation2 {
  0%,
  100% {
    transform: translate(-50%, -50%) scale(1.2);
    top: 75%;
    left: 25%;
  }
  25% {
    transform: translate(-50%, -50%) scale(0.8);
    top: 25%;
    left: 75%;
  }
  50% {
    transform: translate(-50%, -50%) scale(1.5);
    top: 50%;
    left: 50%;
  }
  75% {
    transform: translate(-50%, -50%) scale(1);
    top: 75%;
    left: 25%;
  }
}

@keyframes lensFlareAnimation3 {
  0%,
  100% {
    transform: translate(-50%, -50%) scale(1);
    top: 50%;
    left: 50%;
  }
  25% {
    transform: translate(-50%, -50%) scale(1.5);
    top: 25%;
    left: 25%;
  }
  50% {
    transform: translate(-50%, -50%) scale(0.8);
    top: 75%;
    left: 75%;
  }
  75% {
    transform: translate(-50%, -50%) scale(1.2);
    top: 50%;
    left: 25%;
  }
}

@keyframes lensFlareAnimation4 {
  0%,
  100% {
    transform: translate(-50%, -50%) scale(1.5);
    top: 75%;
    left: 75%;
  }
  25% {
    transform: translate(-50%, -50%) scale(1);
    top: 50%;
    left: 25%;
  }
  50% {
    transform: translate(-50%, -50%) scale(1.2);
    top: 25%;
    left: 75%;
  }
  75% {
    transform: translate(-50%, -50%) scale(0.8);
    top: 50%;
    left: 50%;
  }
}
</style>
