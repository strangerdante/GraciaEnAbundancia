<template>
  <nav
    class="bg-gray-800 bg-opacity-80 backdrop-filter backdrop-blur-sm fixed w-full z-20 top-0 start-0 border-b border-gray-600 navbar selection:bg-teal-500 selection:text-white"
    style="list-style-type: none"
  >
    <div
      class="max-w-screen-xl flex flex-wrap items-center justify-between mx-auto p-4"
    >
      <!-- Logo y nombre de la iglesia -->
      <a
        href="/"
        class="group flex items-center space-x-3 rtl:space-x-reverse relative overflow-hidden rounded-lg p-1"
      >
        <div class="relative">
          <img
            src="https://i.ibb.co/HtyYRq5/logo.png"
            class="h-16 transition-transform duration-300 ease-in-out group-hover:scale-105"
            alt="Logo iglesia"
          />
          <div
            class="absolute inset-0 group-hover:opacity-20 transition-opacity duration-300 ease-in-out"
          ></div>
        </div>
        <span
          class="self-center text-md font-semibold whitespace-nowrap text-white text-center transition-colors duration-300 ease-in-out group-hover:text-teal-400"
          >Iglesia Bautista <br />
          Su Gracia es Mayor</span
        >
      </a>

      <!-- Contenedor de botones (modo oscuro y hamburguesa) -->
      <div class="flex md:order-2 space-x-3 md:space-x-0 rtl:space-x-reverse">
        <!-- Botón de modo oscuro -->
        <button
          @click="toggleDarkMode"
          class="h-10 w-10 rounded-lg p-2 hover:bg-gray-200 dark:hover:bg-gray-700 float-right"
        >
          <!-- Ícono para modo claro -->
          <svg
            class="fill-teal-400 block dark:hidden"
            fill="currentColor"
            viewBox="0 0 20 20"
          >
            <path
              d="M17.293 13.293A8 8 0 016.707 2.707a8.001 8.001 0 1010.586 10.586z"
            ></path>
          </svg>
          <!-- Ícono para modo oscuro -->
          <svg
            class="fill-yellow-500 hidden dark:block"
            fill="currentColor"
            viewBox="0 0 20 20"
          >
            <path
              d="M10 2a1 1 0 011 1v1a1 1 0 11-2 0V3a1 1 0 011-1zm4 8a4 4 0 11-8 0 4 4 0 018 0zm-.464 4.95l.707.707a1 1 0 001.414-1.414l-.707-.707a1 1 0 00-1.414 1.414zm2.12-10.607a1 1 0 010 1.414l-.706.707a1 1 0 11-1.414-1.414l.707-.707a1 1 0 011.414 0zM17 11a1 1 0 100-2h-1a1 1 0 100 2h1zm-7 4a1 1 0 011 1v1a1 1 0 11-2 0v-1a1 1 0 011-1zM5.05 6.464A1 1 0 106.465 5.05l-.708-.707a1 1 0 00-1.414 1.414l.707.707zm1.414 8.486l-.707.707a1 1 0 01-1.414-1.414l.707-.707a1 1 0 011.414 1.414zM4 11a1 1 0 100-2H3a1 1 0 000 2h1z"
              fill-rule="evenodd"
              clip-rule="evenodd"
            ></path>
          </svg>
        </button>

        <!-- Botón de hamburguesa para móviles -->
        <button
          @click="toggleMenu"
          type="button"
          class="inline-flex items-center p-2 w-10 h-10 justify-center text-sm text-gray-500 rounded-lg md:hidden hover:bg-gray-100 focus:outline-none focus:ring-2 focus:ring-gray-200 dark:text-gray-400 dark:hover:bg-gray-700 dark:focus:ring-gray-600"
          aria-controls="navbar-sticky"
          :aria-expanded="menuVisible"
          id="boton-prueba"
        >
          <span class="sr-only">Abrir menu principal</span>
          <!-- Ícono de hamburguesa -->
          <svg
            class="w-5 h-5"
            aria-hidden="true"
            xmlns="http://www.w3.org/2000/svg"
            fill="none"
            viewBox="0 0 17 14"
          >
            <path
              stroke="currentColor"
              stroke-linecap="round"
              stroke-linejoin="round"
              stroke-width="2"
              d="M1 1h15M1 7h15M1 13h15"
            />
          </svg>
        </button>
      </div>

      <!-- Menú de navegación -->
      <div
        :class="[
          'items-center justify-between w-full md:flex md:w-auto md:order-1',
          'absolute md:static top-full left-0 right-0 bg-gray-800 md:bg-transparent bg-opacity-80 backdrop-filter backdrop-blur-sm',
          'pb-6 md:pb-0 p-4 sm:p-0',
          'transition-all duration-300 ease-in-out', // Añadimos estas clases
          menuVisible
            ? 'opacity-100 max-h-screen'
            : 'opacity-0 max-h-0 md:opacity-100 md:max-h-screen',
        ]"
        id="navbar-sticky"
      >
        <!-- Enlaces de navegación -->
        <ul
          class="flex flex-col p-4 md:p-0 mt-4 font-medium border border-gray-100 rounded-lg md:space-x-8 rtl:space-x-reverse md:flex-row md:mt-0 md:border-0 nav-menu"
        >
          <li>
            <a
              href="/#inicio"
              class="ease-in duration-150 block py-2 px-3 text-white rounded md:hover:bg-transparent md:hover:text-teal-400 md:p-0 md:dark:hover:text-teal-400 hover:bg-gray-700 dark:hover:text-white md:dark:hover:bg-transparent dark:border-gray-700"
              >Inicio</a
            >
          </li>
          <li>
            <a
              href="/#pastor"
              class="ease-in duration-150 block py-2 px-3 text-white rounded md:hover:bg-transparent md:hover:text-teal-400 md:p-0 md:dark:hover:text-teal-400 hover:bg-gray-700 dark:hover:text-white md:dark:hover:bg-transparent dark:border-gray-700"
              >Pastor</a
            >
          </li>
          <li>
            <a
              href="/#servicio"
              class="ease-in duration-150 block py-2 px-3 text-white rounded md:hover:bg-transparent md:hover:text-teal-400 md:p-0 md:dark:hover:text-teal-400 hover:bg-gray-700 dark:hover:text-white md:dark:hover:bg-transparent dark:border-gray-700"
              >Servicios</a
            >
          </li>
          <li>
            <a
              href="/#ministerios"
              class="ease-in duration-150 block py-2 px-3 text-white rounded md:hover:bg-transparent md:hover:text-teal-400 md:p-0 md:dark:hover:text-teal-400 hover:bg-gray-700 dark:hover:text-white md:dark:hover:bg-transparent dark:border-gray-700"
              >Ministerios</a
            >
          </li>
          <li>
            <a
              href="/confesion"
              class="ease-in duration-150 block py-2 px-3 text-white rounded md:hover:bg-transparent md:hover:text-teal-400 md:p-0 md:dark:hover:text-teal-400 hover:bg-gray-700 dark:hover:text-white md:dark:hover:bg-transparent dark:border-gray-700"
              >Confesion de fe</a
            >
          </li>
          <li>
            <a
              href="/preguntas"
              class="ease-in duration-150 block py-2 px-3 text-white rounded md:hover:bg-transparent md:hover:text-teal-400 md:p-0 md:dark:hover:text-teal-400 hover:bg-gray-700 dark:hover:text-white md:dark:hover:bg-transparent dark:border-gray-700"
              >Preguntas frecuentes</a
            >
          </li>
        </ul>
      </div>
    </div>
  </nav>

  <BarraProgreso />
  <!-- <BarraProgreso color="#FF5733" /> color personalizado -->
</template>

<script>
import BarraProgreso from "./BarraProgreso.vue";

export default {
  components: {
    BarraProgreso,
  },
  name: "MenuInicio",
  data() {
    return {
      menuVisible: false, // Estado para controlar la visibilidad del menú en móviles
    };
  },
  methods: {
    toggleMenu() {
      this.menuVisible = !this.menuVisible;
      if (this.menuVisible) {
        // Permitimos que el menú se expanda a su altura completa
        this.$nextTick(() => {
          this.menuHeight = `${this.$refs.navMenu.scrollHeight}px`;
        });
      } else {
        // Colapsamos el menú
        this.menuHeight = "0px";
      }
    },
    toggleDarkMode() {
      document.documentElement.classList.toggle("dark");
    },
  },
  mounted() {},
};
</script>

<style></style>
