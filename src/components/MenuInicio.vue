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
          class="self-center text-md whitespace-nowrap text-white text-center transition-colors duration-300 ease-in-out group-hover:text-yellow-500 leading-tight"
        >
          <span class="font-colus">GRACIA</span>
          <br class="leading-[0.5]" />
          <span class="text-sm block -mt-1 font-colus">EN ABUNDANCIA</span>
          <span class="text-xs block -mt-1">Iglesia Bautista</span>
        </span>
      </a>
      <!-- Menú de navegación para pantallas grandes -->
      <div class="hidden md:flex items-center space-x-8">
        <a href="/#inicio" class="text-white hover:text-yellow-400">Inicio</a>
        <div class="relative">
          <button
            class="flex items-center justify-between w-full py-2 px-3 font-medium text-white border-b border-gray-100 md:w-auto hover:bg-gray-50 md:hover:bg-transparent md:border-0 md:hover:text-white md:p-0"
            @click="isOpen = !isOpen"
          >
            Conócenos
            <svg
              class="w-2.5 h-2.5 ms-3 transition-transform duration-200"
              :class="{ 'rotate-180': isOpen }"
              aria-hidden="true"
              xmlns="http://www.w3.org/2000/svg"
              fill="none"
              viewBox="0 0 10 6"
            >
              <path
                stroke="currentColor"
                stroke-linecap="round"
                stroke-linejoin="round"
                stroke-width="2"
                d="m1 1 4 4 4-4"
              />
            </svg>
          </button>
          <div
            v-if="isOpen"
            class="absolute z-10 font-normal bg-white divide-y divide-gray-100 rounded-lg shadow w-44"
          >
            <ul class="py-2 text-sm text-gray-700">
              <li>
                <a href="/#conocenos" class="block px-4 py-2 hover:bg-gray-100"
                  >Pastor</a
                >
              </li>
              <li>
                <a href="/#pastor" class="block px-4 py-2 hover:bg-gray-100"
                  >Ministerios</a
                >
              </li>
              <li>
                <a href="/#ministerio" class="block px-4 py-2 hover:bg-gray-100"
                  >Servicios</a
                >
              </li>
              <li>
                <a href="/#ministerio" class="block px-4 py-2 hover:bg-gray-100"
                  >Galeria</a
                >
              </li>
            </ul>
          </div>
        </div>
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
            class="flex items-center py-2 px-4 text-white hover:bg-blue-800 rounded"
          >
            <svg
              class="w-5 h-5 mr-2"
              fill="none"
              stroke="currentColor"
              viewBox="0 0 24 24"
              xmlns="http://www.w3.org/2000/svg"
            >
              <path
                stroke-linecap="round"
                stroke-linejoin="round"
                stroke-width="2"
                d="M3 12l2-2m0 0l7-7 7 7M5 10v10a1 1 0 001 1h3m10-11l2 2m-2-2v10a1 1 0 01-1 1h-3m-6 0a1 1 0 001-1v-4a1 1 0 011-1h2a1 1 0 011 1v4a1 1 0 001 1m-6 0h6"
              ></path>
            </svg>
            Inicio
          </a>
        </li>
        <li>
          <div
            @click="toggleConocenos"
            class="flex items-center justify-between py-2 px-4 text-white hover:bg-blue-800 rounded cursor-pointer"
          >
            <div class="flex items-center">
              <svg
                class="w-5 h-5 mr-2"
                fill="none"
                stroke="currentColor"
                viewBox="0 0 24 24"
                xmlns="http://www.w3.org/2000/svg"
              >
                <path
                  stroke-linecap="round"
                  stroke-linejoin="round"
                  stroke-width="2"
                  d="M17 20h5v-2a3 3 0 00-5.356-1.857M17 20H7m10 0v-2c0-.656-.126-1.283-.356-1.857M7 20H2v-2a3 3 0 015.356-1.857M7 20v-2c0-.656.126-1.283.356-1.857m0 0a5.002 5.002 0 019.288 0M15 7a3 3 0 11-6 0 3 3 0 016 0zm6 3a2 2 0 11-4 0 2 2 0 014 0zM7 10a2 2 0 11-4 0 2 2 0 014 0z"
                ></path>
              </svg>
              Conócenos
            </div>
            <svg
              :class="{ 'rotate-180': conocenosExpanded }"
              class="w-4 h-4 transition-transform duration-200"
              fill="none"
              stroke="currentColor"
              viewBox="0 0 24 24"
              xmlns="http://www.w3.org/2000/svg"
            >
              <path
                stroke-linecap="round"
                stroke-linejoin="round"
                stroke-width="2"
                d="M19 9l-7 7-7-7"
              ></path>
            </svg>
          </div>
          <ul v-show="conocenosExpanded" class="ml-4 mt-2 space-y-2">
            <li>
              <a
                href="/conocenos/pastor"
                class="flex items-center py-1 px-4 text-white hover:bg-blue-800 rounded"
              >
                <svg
                  class="w-4 h-4 mr-2"
                  fill="none"
                  stroke="currentColor"
                  viewBox="0 0 24 24"
                  xmlns="http://www.w3.org/2000/svg"
                >
                  <path
                    stroke-linecap="round"
                    stroke-linejoin="round"
                    stroke-width="2"
                    d="M16 7a4 4 0 11-8 0 4 4 0 018 0zM12 14a7 7 0 00-7 7h14a7 7 0 00-7-7z"
                  ></path>
                </svg>
                Pastor
              </a>
            </li>
            <li>
              <a
                href="/conocenos/ministerios"
                class="flex items-center py-1 px-4 text-white hover:bg-blue-800 rounded"
              >
                <svg
                  class="w-4 h-4 mr-2"
                  fill="none"
                  stroke="currentColor"
                  viewBox="0 0 24 24"
                  xmlns="http://www.w3.org/2000/svg"
                >
                  <path
                    stroke-linecap="round"
                    stroke-linejoin="round"
                    stroke-width="2"
                    d="M19 11H5m14 0a2 2 0 012 2v6a2 2 0 01-2 2H5a2 2 0 01-2-2v-6a2 2 0 012-2m14 0V9a2 2 0 00-2-2M5 11V9a2 2 0 012-2m0 0V5a2 2 0 012-2h6a2 2 0 012 2v2M7 7h10"
                  ></path>
                </svg>
                Ministerios
              </a>
            </li>
            <li>
              <a
                href="/conocenos/servicios"
                class="flex items-center py-1 px-4 text-white hover:bg-blue-800 rounded"
              >
                <svg
                  class="w-4 h-4 mr-2"
                  fill="none"
                  stroke="currentColor"
                  viewBox="0 0 24 24"
                  xmlns="http://www.w3.org/2000/svg"
                >
                  <path
                    stroke-linecap="round"
                    stroke-linejoin="round"
                    stroke-width="2"
                    d="M21 12a9 9 0 01-9 9m9-9a9 9 0 00-9-9m9 9H3m9 9a9 9 0 01-9-9m9 9c1.657 0 3-4.03 3-9s-1.343-9-3-9m0 18c-1.657 0-3-4.03-3-9s1.343-9 3-9m-9 9a9 9 0 019-9"
                  ></path>
                </svg>
                Servicios
              </a>
            </li>
            <li>
              <a
                href="/conocenos/galeria"
                class="flex items-center py-1 px-4 text-white hover:bg-blue-800 rounded"
              >
                <svg
                  class="w-4 h-4 mr-2"
                  fill="none"
                  stroke="currentColor"
                  viewBox="0 0 24 24"
                  xmlns="http://www.w3.org/2000/svg"
                >
                  <path
                    stroke-linecap="round"
                    stroke-linejoin="round"
                    stroke-width="2"
                    d="M4 16l4.586-4.586a2 2 0 012.828 0L16 16m-2-2l1.586-1.586a2 2 0 012.828 0L20 14m-6-6h.01M6 20h12a2 2 0 002-2V6a2 2 0 00-2-2H6a2 2 0 00-2 2v12a2 2 0 002 2z"
                  ></path>
                </svg>
                Galería
              </a>
            </li>
          </ul>
        </li>
        <li>
          <a
            href="/#anuncios"
            class="flex items-center py-2 px-4 text-white hover:bg-blue-800 rounded"
          >
            <svg
              class="w-5 h-5 mr-2"
              fill="none"
              stroke="currentColor"
              viewBox="0 0 24 24"
              xmlns="http://www.w3.org/2000/svg"
            >
              <path
                stroke-linecap="round"
                stroke-linejoin="round"
                stroke-width="2"
                d="M11 5.882V19.24a1.76 1.76 0 01-3.417.592l-2.147-6.15M18 13a3 3 0 100-6M5.436 13.683A4.001 4.001 0 017 6h1.832c4.1 0 7.625-1.234 9.168-3v14c-1.543-1.766-5.067-3-9.168-3H7a3.988 3.988 0 01-1.564-.317z"
              ></path>
            </svg>
            Anuncios
          </a>
        </li>
        <li>
          <a
            href="/#eventos"
            class="flex items-center py-2 px-4 text-white hover:bg-blue-800 rounded"
          >
            <svg
              class="w-5 h-5 mr-2"
              fill="none"
              stroke="currentColor"
              viewBox="0 0 24 24"
              xmlns="http://www.w3.org/2000/svg"
            >
              <path
                stroke-linecap="round"
                stroke-linejoin="round"
                stroke-width="2"
                d="M8 7V3m8 4V3m-9 8h10M5 21h14a2 2 0 002-2V7a2 2 0 00-2-2H5a2 2 0 00-2 2v12a2 2 0 002 2z"
              ></path>
            </svg>
            Próximos Eventos
          </a>
        </li>
        <li>
          <a
            href="/confesion"
            class="flex items-center py-2 px-4 text-white hover:bg-blue-800 rounded"
          >
            <svg
              class="w-5 h-5 mr-2"
              fill="none"
              stroke="currentColor"
              viewBox="0 0 24 24"
              xmlns="http://www.w3.org/2000/svg"
            >
              <path
                stroke-linecap="round"
                stroke-linejoin="round"
                stroke-width="2"
                d="M12 6.253v13m0-13C10.832 5.477 9.246 5 7.5 5S4.168 5.477 3 6.253v13C4.168 18.477 5.754 18 7.5 18s3.332.477 4.5 1.253m0-13C13.168 5.477 14.754 5 16.5 5c1.747 0 3.332.477 4.5 1.253v13C19.832 18.477 18.247 18 16.5 18c-1.746 0-3.332.477-4.5 1.253"
              ></path>
            </svg>
            Confesión de fe
          </a>
        </li>
        <li>
          <a
            href="/preguntas"
            class="flex items-center py-2 px-4 text-white hover:bg-blue-800 rounded"
          >
            <svg
              class="w-5 h-5 mr-2"
              fill="none"
              stroke="currentColor"
              viewBox="0 0 24 24"
              xmlns="http://www.w3.org/2000/svg"
            >
              <path
                stroke-linecap="round"
                stroke-linejoin="round"
                stroke-width="2"
                d="M8.228 9c.549-1.165 2.03-2 3.772-2 2.21 0 4 1.343 4 3 0 1.4-1.278 2.575-3.006 2.907-.542.104-.994.54-.994 1.093m0 3h.01M21 12a9 9 0 11-18 0 9 9 0 0118 0z"
              ></path>
            </svg>
            Preguntas frecuentes
          </a>
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
      isOpen: false,
      conocenosExpanded: false,
    };
  },
  methods: {
    toggleSidebar() {
      this.sidebarVisible = !this.sidebarVisible;
    },
    toggleConocenos() {
      this.conocenosExpanded = !this.conocenosExpanded;
    },
  },
};
</script>
