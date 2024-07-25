<template>
  <!-- Barra de navegación principal -->
  <nav
    class="bg-blue-900 fixed w-full z-30 top-0 start-0 border-b border-gray-600 navbar"
  >
    <div class="max-w-screen-xl flex items-center justify-between mx-auto p-4">
      <!-- Logo y nombre de la iglesia -->
      <a
        href="/"
        class="group flex items-center space-x-3 rtl:space-x-reverse relative overflow-hidden rounded-lg p-1"
      >
        <div class="relative">
          <img
            src="https://i.ibb.co/rys80PM/ICONO.png"
            class="h-16 transition-transform duration-300 ease-in-out group-hover:scale-105"
            alt="Logo iglesia"
          />
          <div
            class="absolute inset-0 group-hover:opacity-20 transition-opacity duration-300 ease-in-out"
          ></div>
        </div>
        <span
          class="self-center text-md font-semibold whitespace-nowrap text-white text-center transition-colors duration-300 ease-in-out group-hover:text-yellow-500 leading-tight"
        >
          GRACIA
          <br class="leading-[0.5]" />
          <span class="text-sm block -mt-1">EN ABUNDANCIA</span>
          <span class="text-xs block -mt-1">Iglesia Bautista</span>
        </span>
      </a>

      <!-- Menú de navegación para pantallas grandes -->
      <div class="hidden md:flex items-center space-x-8">
        <a href="/#inicio" class="text-white hover:text-yellow-400">Inicio</a>
        <a href="/#conocenos" class="text-white hover:text-yellow-400"
          >Conocenos</a
        >
        <a href="/#anuncios" class="text-white hover:text-yellow-400"
          >Anuncios</a
        >
        <a href="/#eventos" class="text-white hover:text-yellow-400"
          >Proximos Eventos</a
        >
        <a href="/confesion" class="text-white hover:text-yellow-400"
          >Confesión de fe</a
        >
        <a href="/preguntas" class="text-white hover:text-yellow-400"
          >Preguntas frecuentes</a
        >
      </div>

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
            href="/#conocenos"
            class="block py-2 px-4 text-white hover:bg-blue-800 rounded"
            >Conocenos</a
          >
        </li>
        <li>
          <a
            href="/#servicio"
            class="block py-2 px-4 text-white hover:bg-blue-800 rounded"
            >Anuncios</a
          >
        </li>
        <li>
          <a
            href="/#ministerios"
            class="block py-2 px-4 text-white hover:bg-blue-800 rounded"
            >Proximos Eventos</a
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
