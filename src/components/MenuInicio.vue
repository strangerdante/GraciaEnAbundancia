<template>
  <!-- Barra de navegación principal -->
  <nav
    class="bg-blue-900 fixed w-full z-30 top-0 start-0 border-b border-gray-600 navbar"
  >
    <div class="max-w-screen-xl flex items-center justify-between mx-auto p-4">
      <!-- Logo y nombre de la iglesia -->
      <a href="/" class="flex items-center space-x-3 rtl:space-x-reverse">
        <img
          src="https://i.ibb.co/HtyYRq5/logo.png"
          class="h-16"
          alt="Logo iglesia"
        />
        <span
          class="self-center text-md font-semibold whitespace-nowrap text-white"
        >
          Iglesia Bautista <br />
          Gracia en abundancia
        </span>
      </a>

      <!-- Menú de navegación para pantallas grandes -->
      <div class="hidden md:flex items-center space-x-8">
        <a href="/#inicio" class="text-white hover:text-yellow-400">Inicio</a>
        <a href="/#pastor" class="text-white hover:text-yellow-400">Pastor</a>
        <a href="/#servicio" class="text-white hover:text-yellow-400"
          >Servicios</a
        >
        <a href="/#ministerios" class="text-white hover:text-yellow-400"
          >Ministerios</a
        >
        <a href="/confesion" class="text-white hover:text-yellow-400"
          >Confesión de fe</a
        >
        <a href="/preguntas" class="text-white hover:text-yellow-400"
          >Preguntas frecuentes</a
        >
      </div>

      <!-- Botón de modo oscuro -->
      <button
        @click="toggleDarkMode"
        class="h-10 w-10 rounded-lg p-2 hover:bg-gray-200"
      >
        <svg v-if="!isDarkMode" class="fill-blue-400" viewBox="0 0 20 20">
          <path
            d="M17.293 13.293A8 8 0 016.707 2.707a8.001 8.001 0 1010.586 10.586z"
          ></path>
        </svg>
        <svg v-else class="fill-yellow-500" viewBox="0 0 20 20">
          <path
            d="M10 2a1 1 0 011 1v1a1 1 0 11-2 0V3a1 1 0 011-1zm4 8a4 4 0 11-8 0 4 4 0 018 0zm-.464 4.95l.707.707a1 1 0 001.414-1.414l-.707-.707a1 1 0 00-1.414 1.414zm2.12-10.607a1 1 0 010 1.414l-.706.707a1 1 0 11-1.414-1.414l.707-.707a1 1 0 011.414 0zM17 11a1 1 0 100-2h-1a1 1 0 100 2h1zm-7 4a1 1 0 011 1v1a1 1 0 11-2 0v-1a1 1 0 011-1zM5.05 6.464A1 1 0 106.465 5.05l-.708-.707a1 1 0 00-1.414 1.414l.707.707zm1.414 8.486l-.707.707a1 1 0 01-1.414-1.414l.707-.707a1 1 0 011.414 1.414zM4 11a1 1 0 100-2H3a1 1 0 000 2h1z"
            fill-rule="evenodd"
            clip-rule="evenodd"
          ></path>
        </svg>
      </button>

      <!-- Botón de hamburguesa para móviles -->
      <button
        @click="toggleSidebar"
        class="md:hidden inline-flex items-center p-2 w-10 h-10 justify-center text-sm text-white rounded-lg hover:bg-blue-800 focus:outline-none focus:ring-2 focus:ring-gray-200"
      >
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
  </nav>

  <!-- Sidebar para móviles -->
  <div
    :class="[
      'fixed inset-y-0 left-0 z-40 w-64 bg-blue-900 overflow-y-auto transition-transform duration-300 ease-in-out transform md:hidden',
      sidebarVisible ? 'translate-x-0' : '-translate-x-full',
    ]"
  >
    <!-- Contenido del sidebar -->
    <div class="p-5">
      <button
        @click="toggleSidebar"
        class="absolute top-4 right-4 text-white hover:text-yellow-400"
      >
        <svg
          class="w-6 h-6"
          fill="none"
          stroke="currentColor"
          viewBox="0 0 24 24"
          xmlns="http://www.w3.org/2000/svg"
        >
          <path
            stroke-linecap="round"
            stroke-linejoin="round"
            stroke-width="2"
            d="M6 18L18 6M6 6l12 12"
          ></path>
        </svg>
      </button>
      <ul class="space-y-4 mt-8">
        <li>
          <a
            href="/#inicio"
            class="block py-2 px-4 text-white hover:bg-blue-800 rounded"
            >Inicio</a
          >
        </li>
        <li>
          <a
            href="/#pastor"
            class="block py-2 px-4 text-white hover:bg-blue-800 rounded"
            >Pastor</a
          >
        </li>
        <li>
          <a
            href="/#servicio"
            class="block py-2 px-4 text-white hover:bg-blue-800 rounded"
            >Servicios</a
          >
        </li>
        <li>
          <a
            href="/#ministerios"
            class="block py-2 px-4 text-white hover:bg-blue-800 rounded"
            >Ministerios</a
          >
        </li>
        <li>
          <a
            href="/confesion"
            class="block py-2 px-4 text-white hover:bg-blue-800 rounded"
            >Confesión de fe</a
          >
        </li>
        <li>
          <a
            href="/preguntas"
            class="block py-2 px-4 text-white hover:bg-blue-800 rounded"
            >Preguntas frecuentes</a
          >
        </li>
      </ul>
    </div>
  </div>

  <!-- Overlay para cerrar el sidebar al hacer clic fuera -->
  <div
    v-if="sidebarVisible"
    @click="toggleSidebar"
    class="fixed inset-0 bg-black bg-opacity-50 z-30 md:hidden"
  ></div>

  <!-- Componente de barra de progreso -->
  <BarraProgreso />
</template>

<script>
import BarraProgreso from "./BarraProgreso.vue";

export default {
  components: { BarraProgreso },
  name: "MenuInicio",
  data() {
    return {
      sidebarVisible: false,
      isDarkMode: false,
    };
  },
  methods: {
    toggleSidebar() {
      this.sidebarVisible = !this.sidebarVisible;
    },
    toggleDarkMode() {
      this.isDarkMode = !this.isDarkMode;
      document.documentElement.classList.toggle("dark");
      localStorage.setItem("darkMode", this.isDarkMode);
    },
    loadDarkModePreference() {
      const darkMode = localStorage.getItem("darkMode") === "true";
      this.isDarkMode = darkMode;
      if (darkMode) {
        document.documentElement.classList.add("dark");
      }
    },
  },
  mounted() {
    this.loadDarkModePreference();
  },
};
</script>
