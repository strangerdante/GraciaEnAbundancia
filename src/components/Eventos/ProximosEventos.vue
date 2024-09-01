<template>
  <div
    class="bg-gray-100 dark:bg-slate-600 px-1 sm:px-6 2xl:px-80 pb-10 transition duration-300 ease-in-out selection:bg-teal-500 selection:text-white"
  >
    <div class="flex justify-between items-center mb-8">
      <h2 class="text-3xl font-bold dark:text-white">Próximos eventos</h2>
      <button
        @click="toggleView"
        class="text-2xl text-gray-700 dark:text-teal-500 mr-1"
      >
        <i :class="isCarouselView ? 'fas fa-list' : 'fas fa-th'"></i>
      </button>
    </div>

    <!-- Estado de carga -->
    <div v-if="cargando" class="flex flex-col justify-center items-center h-64">
      <div class="loader mb-4"></div>
      <p class="text-gray-700 dark:text-gray-300">Cargando eventos...</p>
    </div>

    <!-- Mensaje de error -->
    <div v-else-if="error" class="text-red-500 text-center py-8">
      {{ error }}
    </div>

    <!-- Contenido principal -->
    <div v-else>
      <ModoCarrusel
        v-if="isCarouselView"
        :eventos="eventos"
        @abrir-modal="abrirModal"
      />
      <ModoLista
        v-else
        :eventos="eventos"
        :proximoEvento="proximoEvento"
        :eventosAdicionales="eventosAdicionales"
        @abrir-modal="abrirModal"
      />
    </div>

    <EventoModal
      v-if="eventoSeleccionado"
      :evento="eventoSeleccionado"
      @cerrar="cerrarModal"
    />
  </div>
</template>

<script>
import { ref, computed, onMounted } from "vue";
import ModoCarrusel from "./ModoCarrusel.vue";
import ModoLista from "./ModoLista.vue";
import EventoModal from "./EventoModal.vue";

export default {
  components: {
    ModoCarrusel,
    ModoLista,
    EventoModal,
  },
  setup() {
    const eventos = ref([]);
    const eventoSeleccionado = ref(null);
    const cargando = ref(true);
    const error = ref(null);
    const isCarouselView = ref(true);

    const toggleView = () => {
      isCarouselView.value = !isCarouselView.value;
    };

    const abrirModal = (evento) => {
      eventoSeleccionado.value = evento;
    };

    const cerrarModal = () => {
      eventoSeleccionado.value = null;
    };

    const obtenerDiaSemana = (fecha) => {
      const dias = [
        "Domingo",
        "Lunes",
        "Martes",
        "Miércoles",
        "Jueves",
        "Viernes",
        "Sábado",
      ];
      return dias[fecha.getUTCDay()];
    };

    const proximoEvento = computed(() => {
      return eventos.value[0] || null;
    });

    const eventosAdicionales = computed(() => {
      return eventos.value.slice(1, 4);
    });

    const generarEventos = (startDate, endDate) => {
      // Genera eventos recurrentes para el período especificado (8 días)
      const events = [];
      const currentDate = new Date(startDate);

      while (currentDate <= endDate) {
        const year = currentDate.getUTCFullYear();
        const month = currentDate.getUTCMonth();
        const date = currentDate.getUTCDate();
        const day = currentDate.getUTCDay();

        // Eventos dominicales
        if (day === 0) {
          const weekOfMonth = Math.floor((date - 1) / 7) + 1;
          let infoIconoTexto = "Servicio dominical";
          let banner = "https://i.ibb.co/LzmwPvs/domingos.jpg";
          let descripcion = "Servicio dominical semanal.";

          if (weekOfMonth === 1) {
            infoIconoTexto = "Cena del Señor"; // Cena del Señor
            banner = "https://i.ibb.co/j5XSjXD/cena.jpg";
            descripcion =
              "Celebración mensual de la Cena del Señor. Un tiempo especial de comunión y remembranza.";
          } else if (weekOfMonth === 3) {
            infoIconoTexto = "Canasta de amor"; // Canasta de amor
            banner =
              "https://i.ibb.co/prVxsvq/ccb02c5d-aca0-4a1a-baa9-501c57e017c8.jpg";
            descripcion =
              "Domingo de ofrenda especial para apoyar a familias necesitadas de nuestra comunidad.";
          } else if (date > 24) {
            infoIconoTexto = "Domingo misionero"; // Domingo misionero
            banner = null;
            descripcion =
              "Enfoque especial en nuestras misiones y misioneros alrededor del mundo.";
          }
          // Eventos domingos
          events.push({
            id: `domingo-${year}-${month + 1}-${date}`,
            fecha: new Date(Date.UTC(year, month, date)),
            titulo: "Servicio dominical",
            hora: "8:00 am — 12:30 pm",
            lugar: "Carrera 35 #1C-30",
            descripcion: descripcion,
            infoAdiccional: true,
            infoIconoTexto: infoIconoTexto,
            banner: banner,
          });
        }

        // Eventos del último sábado del mes
        if (day === 6 && date > 24) {
          events.push({
            id: `varones-${year}-${month + 1}-${date}`,
            fecha: new Date(Date.UTC(year, month, date)),
            titulo: "Reunión de varones",
            hora: "3:30 pm - 5:30 pm",
            lugar: "Por confirmar",
            descripcion: "Reunión mensual de varones.",
            infoAdiccional: true,
            infoIconoTexto: "Reunión de varones",
            banner: null,
          });

          events.push({
            id: `damas-${year}-${month + 1}-${date}`,
            fecha: new Date(Date.UTC(year, month, date)),
            titulo: "Reunión de damas",
            hora: "3:30 pm - 5:30 pm",
            lugar: "Por confirmar",
            descripcion: "Reunión mensual de damas.",
            infoAdiccional: true,
            infoIconoTexto: "Reunión de damas",
            banner: null,
          });
        }

        // Eventos de los miércoles
        if (day === 3) {
          events.push({
            id: `oracion-${year}-${month + 1}-${date}`,
            fecha: new Date(Date.UTC(year, month, date)),
            titulo: "Culto de oración",
            hora: "7:00 pm - 8:00 pm",
            link: "https://acortar.link/CultoDeOracionGAIB",
            descripcion: "Culto semanal de oración. ",
            infoAdiccional: true,
            infoIconoTexto: "Culto de oración",
            banner: "https://i.ibb.co/bWTbtrv/culto-oracion.jpg",
          });
        }

        // Cumple iglesia
        if (month === 7 && date === 12) {
          // Nota: mes 7 es agosto en JavaScript
          events.push({
            id: `cumple-${year}-8-12`,
            fecha: new Date(Date.UTC(year, month, date)),
            titulo: "Cumple iglesia",
            hora: "7:00 pm - 8:00 pm",
            lugar: "Carrera 35 #1C-30",
            descripcion:
              "Celebramos un año más del nacimiento de nuestra iglesia",
            infoAdiccional: true,
            infoIconoTexto: "Cumple",
            banner: null,
            confeti: true, // Nueva propiedad para activar el confeti
          });
        }

        currentDate.setUTCDate(currentDate.getUTCDate() + 1);
      }

      return events;
    };

    const fusionarEventos = (eventosGenerados, eventosAPI) => {
      const eventosMap = new Map();

      // Agrega todos los eventos generados al mapa
      eventosGenerados.forEach((evento) => {
        const key = `${evento.fecha.toISOString().split("T")[0]}-${evento.id}`;
        eventosMap.set(key, evento);
      });

      // Sobreescribe o agrega los eventos de la API
      eventosAPI.forEach((eventoAPI) => {
        const fecha = new Date(eventoAPI.fecha);
        const key = `${eventoAPI.fecha}-${eventoAPI.id || ""}`;

        if (eventosMap.has(key)) {
          // Sobreescribe el evento existente
          eventosMap.set(key, {
            ...eventosMap.get(key),
            ...eventoAPI,
            fecha: fecha,
          });
        } else {
          // Agrega el nuevo evento de la API
          eventosMap.set(key, {
            ...eventoAPI,
            fecha: fecha,
          });
        }
      });

      // Convierte el mapa de vuelta a un array y ordena los eventos
      return Array.from(eventosMap.values()).sort(
        (a, b) => a.fecha - b.fecha || a.titulo.localeCompare(b.titulo)
      );
    };

    onMounted(async () => {
      try {
        cargando.value = true;
        const today = new Date();
        today.setUTCHours(0, 0, 0, 0);
        const endDate = new Date(today);
        endDate.setUTCDate(endDate.getUTCDate() + 15); // Eventos de 15 días

        const eventosGenerados = generarEventos(today, endDate);

        // Obtiene eventos de la API
        const respuesta = await fetch("/eventos.json");
        if (!respuesta.ok) {
          throw new Error("Error al obtener los eventos");
        }
        const eventosAPI = await respuesta.json();

        // Fusiona los eventos generados con los de la API
        const eventosFusionados = fusionarEventos(eventosGenerados, eventosAPI);

        // Procesa los eventos fusionados y filtra los eventos antiguos
        eventos.value = eventosFusionados
          .map((evento) => {
            const fechaEvento = new Date(evento.fecha);
            const diasRestantes = Math.ceil(
              (fechaEvento - today) / (1000 * 60 * 60 * 24)
            );

            return {
              ...evento,
              fecha: fechaEvento,
              dia: fechaEvento.getUTCDate().toString().padStart(2, "0"),
              mes: fechaEvento.toLocaleString("es", {
                month: "long",
                timeZone: "UTC",
              }),
              diasRestantes: diasRestantes,
              diaSemana: obtenerDiaSemana(fechaEvento),
            };
          })
          .filter((evento) => evento.diasRestantes >= -1) // Filtra eventos con diasRestantes >= -1
          .sort(
            (a, b) => a.fecha - b.fecha || a.titulo.localeCompare(b.titulo)
          );
      } catch (err) {
        console.error("Error al cargar los eventos:", err);
        error.value =
          "No se pudieron cargar los eventos. Por favor, intente más tarde.";
      } finally {
        cargando.value = false;
      }
    });

    return {
      eventos,
      eventoSeleccionado,
      abrirModal,
      cerrarModal,
      cargando,
      error,
      obtenerDiaSemana,
      isCarouselView,
      toggleView,
      proximoEvento,
      eventosAdicionales,
    };
  },
};
</script>

<style scoped>
.loader {
  width: 50px;
  padding: 8px;
  aspect-ratio: 1;
  border-radius: 50%;
  background: #3b82f6;
  --_m: conic-gradient(#0000 10%, #000), linear-gradient(#000 0 0) content-box;
  -webkit-mask: var(--_m);
  mask: var(--_m);
  -webkit-mask-composite: source-out;
  mask-composite: subtract;
  animation: l3 1s infinite linear;
}
@keyframes l3 {
  to {
    transform: rotate(1turn);
  }
}
</style>
