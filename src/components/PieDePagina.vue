<template>
  <footer
    class="w-full py-5 sm:py-10 px-4 relative bg-cover bg-center bg-no-repeat text-gray-200"
    style="
      background-image: url('https://i.ibb.co/DzcDqP3/Captura-de-pantalla-2024-07-24-235010.png');
    "
  >
    <div class="absolute inset-0 bg-blue-900 opacity-90"></div>
    <div class="relative z-10">
      <h2 class="sr-only">Footer</h2>
      <div
        class="flex flex-col-reverse sm:flex-row md:justify-between lg:justify-around"
      >
        <div
          class="relative mt-14 sm:mt-0 px-5 flex flex-col justify-center items-center text-gray-200"
        >
          <h1 class="font-title text-center leading-tight">
            <span class="block text-4xl sm:text-5xl md:text-6xl font-colus"
              >GRACIA</span
            >
            <span class="block text-xl sm:text-2xl md:text-3xl -mt-1 font-colus"
              >EN ABUNDANCIA</span
            >
            <span class="block text-sm sm:text-base md:text-lg mb-2"
              >Iglesia Bautista</span
            >
          </h1>
          <blockquote class="text-center">
            "Porque por gracia sois salvos por medio de la fe; y esto no de
            vosotros, pues es don de Dios; <br />
            no por obras, para que nadie se gloríe.<br />
            Efesios 2:8-9
          </blockquote>

          <div class="w-full h-px bg-gray-600 my-6"></div>

          <div class="mt-auto flex flex-col items-center">
            <span
              class="inline-flex mt-6 w-full justify-center md:justify-start md:w-auto"
            >
              <a
                href="https://www.facebook.com/GraciaEnAbundanciaIB"
                class="text-gray-200 hover:text-blue-400 transition duration-300"
              >
                <span class="sr-only">Facebook</span>
                <svg
                  fill="currentColor"
                  stroke-linecap="round"
                  stroke-linejoin="round"
                  stroke-width="2"
                  class="w-6 h-6"
                  viewBox="0 0 24 24"
                >
                  <path
                    d="M18 2h-3a5 5 0 00-5 5v3H7v4h3v8h4v-8h3l1-4h-4V7a1 1 0 011-1h3z"
                  ></path>
                </svg>
              </a>
              <a
                href="https://youtube.com/@graciaenabundancia_ib"
                class="ml-3 text-gray-200 hover:text-blue-400 transition duration-300"
              >
                <span class="sr-only">YouTube</span>
                <svg
                  fill="currentColor"
                  stroke-linecap="round"
                  stroke-linejoin="round"
                  stroke-width="2"
                  class="w-6 h-6"
                  viewBox="0 0 24 24"
                >
                  <path
                    d="M21.2,7.7c-0.3-1.1-0.9-2-1.7-2.8C18.7,4.6,16.7,4,12,4S5.3,4.6,3.5,4.9C2.7,5.7,2.1,6.6,1.8,7.7C1.5,8.9,1.5,10.4,1.5,12s0,3.1,0.3,4.3c0.3,1.1,0.9,2,1.7,2.8C5.3,19.4,7.3,20,12,20s6.7-0.6,8.5-0.9c0.8-0.7,1.4-1.7,1.7-2.8c0.3-1.1,0.3-2.6,0.3-4.3s0-3.1-0.3-4.3zM9.8,15.3V8.7l5.6,3.3L9.8,15.3z"
                  ></path>
                </svg>
              </a>
              <a
                href="https://instagram.com/graciaenabundancia_ib"
                class="ml-3 text-gray-200 hover:text-blue-400 transition duration-300"
              >
                <span class="sr-only">Instagram</span>
                <svg
                  fill="none"
                  stroke="currentColor"
                  stroke-linecap="round"
                  stroke-linejoin="round"
                  stroke-width="2"
                  class="w-6 h-6"
                  viewBox="0 0 24 24"
                >
                  <rect width="20" height="20" x="2" y="2" rx="5" ry="5"></rect>
                  <path
                    d="M16 11.37A4 4 0 1112.63 8 4 4 0 0116 11.37zm1.5-4.87h.01"
                  ></path>
                </svg>
              </a>
            </span>
            <span class="py-4 text-xs"
              >&copy;2024, Todos los derechos reservados.</span
            >
          </div>

          <span
            class="sm:hidden absolute -top-4 left-1/2 w-1/4 h-px bg-gray-400 transform -translate-x-1/2"
            aria-hidden="true"
          />
        </div>
        <div class="grid grid-cols-2 gap-5 text-gray-300">
          <div
            class="col-span-2 md:col-span-1 pb-0 md:py-3 px-4 flex flex-col items-center sm:items-start"
          >
            <h3
              class="py-1.5 md:py-4 text-xl text-white font-bold tracking-wide flex items-center"
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
                  d="M4 6h16M4 12h16M4 18h16"
                ></path>
              </svg>
              Navegación
            </h3>
            <div class="w-full h-px bg-gray-600 mb-4"></div>
            <nav
              class="flex justify-around md:flex-col font-medium list-none ml-4 gap-2"
            >
              <li>
                <a href="#inicio" class="hover:text-yellow-500">Inicio</a>
              </li>
              <li>
                <a href="#conocenos" class="hover:text-yellow-500">Conocenos</a>
              </li>
              <li>
                <a href="#anuncios" class="hover:text-yellow-500">Anuncios</a>
              </li>
              <li>
                <a href="#eventos" class="hover:text-yellow-500">Eventos</a>
              </li>
              <li>
                <a
                  href="confesion.html"
                  class="hover:text-yellow-500 hidden lg:block"
                  >Confesion de fe</a
                >
              </li>
              <li>
                <a
                  href="preguntas.html"
                  class="hover:text-yellow-500 hidden lg:block"
                  >Preguntas frecuentes</a
                >
              </li>
            </nav>
          </div>

          <div
            class="col-span-2 md:col-span-1 pb-0 md:py-3 px-4 flex flex-col items-center sm:items-start"
          >
            <h3
              class="py-1.5 md:py-4 text-xl text-white font-bold tracking-wide flex items-center"
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
                  d="M13 16h-1v-4h-1m1-4h.01M21 12a9 9 0 11-18 0 9 9 0 0118 0z"
                ></path>
              </svg>
              Información
            </h3>
            <div class="w-full h-px bg-gray-600 mb-4"></div>
            <ol>
              <li class="flex items-start mb-4">
                <svg
                  class="w-5 h-5 mr-2 mt-1"
                  fill="none"
                  stroke="currentColor"
                  viewBox="0 0 24 24"
                  xmlns="http://www.w3.org/2000/svg"
                >
                  <path
                    stroke-linecap="round"
                    stroke-linejoin="round"
                    stroke-width="2"
                    d="M17.657 16.657L13.414 20.9a1.998 1.998 0 01-2.827 0l-4.244-4.243a8 8 0 1111.314 0z"
                  ></path>
                  <path
                    stroke-linecap="round"
                    stroke-linejoin="round"
                    stroke-width="2"
                    d="M15 11a3 3 0 11-6 0 3 3 0 016 0z"
                  ></path>
                </svg>
                <span>
                  Carrera 35 #1C-30 <br />
                  Bogotá, Colombia
                </span>
              </li>
              <li class="flex items-start">
                <svg
                  class="w-5 h-5 mr-2 mt-1"
                  fill="none"
                  stroke="currentColor"
                  viewBox="0 0 24 24"
                  xmlns="http://www.w3.org/2000/svg"
                >
                  <path
                    stroke-linecap="round"
                    stroke-linejoin="round"
                    stroke-width="2"
                    d="M12 8v4l3 3m6-3a9 9 0 11-18 0 9 9 0 0118 0z"
                  ></path>
                </svg>
                <span>
                  Servicios de Adoración: <br />Domingos 8:00 am - <br />
                  12:00 pm</span
                >
              </li>
            </ol>
          </div>
        </div>
      </div>
    </div>
  </footer>
</template>

<script></script>
