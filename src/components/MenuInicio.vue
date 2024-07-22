<!-- src/App.vue -->
<template>
  <nav
    class="bg-gray-800 bg-opacity-80 backdrop-filter backdrop-blur-sm fixed w-full z-20 top-0 start-0 border-b border-gray-600 navbar"
    style="list-style-type: none"
  >
    <!-- glassmorphing en menu -->
    <div
      class="max-w-screen-xl flex flex-wrap items-center justify-between mx-auto p-4"
    >
      <a href="/" class="flex items-center space-x-3 rtl:space-x-reverse">
        <img
          src="https://i.ibb.co/HtyYRq5/logo.png"
          class="h-12"
          alt="Logo iglesia"
        />
        <span
          class="self-center text-md font-semibold whitespace-nowrap text-white text-center"
          >Iglesia Bautista <br />
          Su Gracia es Mayor</span
        >
      </a>
      <div class="flex md:order-2 space-x-3 md:space-x-0 rtl:space-x-reverse">
        <!-- boton modo oscuro -->
        <button
          onclick="(() => document.documentElement.classList.toggle('dark'))()"
          class="h-10 w-10 rounded-lg p-2 hover:bg-gray-200 dark:hover:bg-gray-700 float-right"
        >
          <svg
            class="fill-teal-400 block dark:hidden"
            fill="currentColor"
            viewBox="0 0 20 20"
          >
            <path
              d="M17.293 13.293A8 8 0 016.707 2.707a8.001 8.001 0 1010.586 10.586z"
            ></path>
          </svg>
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

        <!-- boton hamburguesa -->
        <button
          data-collapse-toggle="navbar-sticky"
          type="button"
          class="inline-flex items-center p-2 w-10 h-10 justify-center text-sm text-gray-500 rounded-lg md:hidden hover:bg-gray-100 focus:outline-none focus:ring-2 focus:ring-gray-200 dark:text-gray-400 dark:hover:bg-gray-700 dark:focus:ring-gray-600"
          aria-controls="navbar-sticky"
          aria-expanded="false"
          id="boton-prueba"
        >
          <span class="sr-only">Abrir menu principal</span>
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

      <div
        class="items-center justify-between hidden w-full md:flex md:w-auto md:order-1"
        id="navbar-sticky"
      >
        <ul
          class="flex flex-col p-4 md:p-0 mt-10 font-medium border border-gray-100 rounded-lg md:space-x-8 rtl:space-x-reverse md:flex-row md:mt-0 md:border-0 nav-menu"
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
</template>

<script>
import BarraProgreso from "./BarraProgreso.vue";

export default {
  components: {
    BarraProgreso,
  },
  name: "MenuInicio",
  mounted() {
    const script = document.createElement("script");
    script.src =
      "https://cdnjs.cloudflare.com/ajax/libs/flowbite/2.2.1/flowbite.min.js";
    document.head.appendChild(script);
  },
};
</script>
<style>
/* Estilos generales */
</style>
