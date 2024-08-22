<template>
  <div class="container mx-auto px-2 py-8 lg:px-32">
    <div v-if="error" class="text-red-500 text-center">{{ error }}</div>
    <!-- Estado de carga -->
    <div
      v-else-if="loading"
      class="flex flex-col justify-center items-center h-64"
    >
      <div class="loader mb-4"></div>
      <p class="text-gray-700 dark:text-gray-300">Cargando videos...</p>
    </div>
    <div v-else>
      <!-- Video destacado -->
      <div v-if="videos.length > 0" class="mb-8">
        <div
          class="flex flex-col md:flex-row bg-white rounded-lg shadow-md overflow-hidden cursor-pointer relative"
          @click="openVideo(videos[0].id)"
        >
          <div class="relative">
            <img
              :src="videos[0].thumbnail"
              :alt="videos[0].title"
              class="w-full h-46 object-cover rounded-lg"
            />
            <div
              class="absolute bottom-2 right-2 bg-black bg-opacity-75 text-white px-2 py-1 rounded-md text-sm"
            >
              {{ videos[0].duration }}
            </div>
          </div>
          <div class="md:w-1/2 p-6">
            <h2 class="text-2xl font-bold mb-4">{{ videos[0].title }}</h2>
            <p class="text-gray-600 mb-4">{{ videos[0].description }}</p>
            <p class="text-sm text-gray-500">{{ videos[0].date }}</p>
          </div>
        </div>
      </div>
      <!-- Carrusel de videos -->
      <h3 class="text-xl font-bold mb-4">Últimos videos</h3>
      <swiper
        :modules="modulos"
        :slides-per-view="2"
        :space-between="4"
        :pagination="{ clickable: true }"
        :navigation="false"
        :grab-cursor="false"
        class="mySwiper custom-swiper rounded-lg overflow-hidden pb-10"
        :breakpoints="{
          '420': {
            slidesPerView: 2,
            spaceBetween: 7,
          },
          '768': {
            slidesPerView: 3,
            spaceBetween: 20,
          },
          '1020': {
            slidesPerView: 4,
            spaceBetween: 20,
          },
        }"
      >
        <swiper-slide v-for="video in videos.slice(1)" :key="video.id">
          <div
            class="bg-white rounded-lg shadow-md overflow-hidden mb-12 h-80 sm:h-[365px] xl:h-[400px] cursor-pointer relative"
            @click="openVideo(video.id)"
          >
            <div class="relative">
              <img
                :src="video.thumbnail"
                :alt="video.title"
                class="w-full h-46 object-cover"
              />
              <div
                class="absolute bottom-2 right-2 bg-black bg-opacity-75 text-white px-2 py-1 rounded-md text-sm"
              >
                {{ video.duration }}
              </div>
            </div>
            <div class="px-2 pt-4">
              <h4
                class="text-sm line-clamp-3 sm:text-[16px] font-semibold mb-2"
              >
                {{ video.title }}
              </h4>
              <p class="text-gray-600 text-sm mb-2">{{ video.author }}</p>
              <p class="text-gray-500 text-xs">{{ video.date }}</p>
            </div>
          </div>
        </swiper-slide>
      </swiper>
    </div>

    <!-- Modal de video -->
    <div
      v-if="showModal"
      class="fixed inset-0 backdrop-blur-sm bg-gray-900/90 flex items-center justify-center z-50"
    >
      <div class="p-14 px-4 rounded-lg w-full max-w-3xl relative">
        <button
          @click="closeModal"
          class="absolute top-2 right-2 p-1 bg-gray-200 rounded-full hover:bg-gray-300 transition duration-300"
          title="Cerrar"
        >
          <svg
            xmlns="http://www.w3.org/2000/svg"
            class="h-5 w-5"
            fill="none"
            viewBox="0 0 24 24"
            stroke="currentColor"
          >
            <path
              stroke-linecap="round"
              stroke-linejoin="round"
              stroke-width="2"
              d="M6 18L18 6M6 6l12 12"
            />
          </svg>
        </button>
        <div class="relative pb-[56.25%] h-0">
          <iframe
            :src="`https://www.youtube.com/embed/${currentVideoId}`"
            class="absolute top-0 left-0 w-full h-full rounded-xl"
            frameborder="0"
            allow="autoplay; encrypted-media"
            allowfullscreen
          ></iframe>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { Swiper, SwiperSlide } from "swiper/vue";
import "swiper/css";
import "swiper/css/navigation";
import "swiper/css/pagination";
import { Navigation, Pagination } from "swiper/modules";

export default {
  components: {
    Swiper,
    SwiperSlide,
  },
  data() {
    return {
      apiKey: "AIzaSyCqHZk1vP6pAVMIuPWpzdzfB92SCLbF-d4",
      channelId: "UCPP7f0ZyCyZo8I_qgwFqveQ",
      videos: [],
      error: null,
      loading: true,
      modulos: [Navigation, Pagination],
      showModal: false,
      currentVideoId: null,
    };
  },
  created() {
    this.fetchVideos();
  },
  methods: {
    async fetchVideos() {
      try {
        this.loading = true;
        const response = await fetch(
          `https://www.googleapis.com/youtube/v3/search?key=${this.apiKey}&channelId=${this.channelId}&part=snippet&type=video&order=date&maxResults=7`
        );
        const searchData = await response.json();
        if (searchData.error) {
          throw new Error(searchData.error.message);
        }

        // Obtener los IDs de los videos
        const videoIds = searchData.items
          .map((item) => item.id.videoId)
          .join(",");

        // Hacer una segunda llamada para obtener la duración de los videos
        const contentDetailsResponse = await fetch(
          `https://www.googleapis.com/youtube/v3/videos?key=${this.apiKey}&id=${videoIds}&part=contentDetails`
        );
        const contentDetailsData = await contentDetailsResponse.json();

        // Combinar los datos
        this.videos = searchData.items.map((item, index) => ({
          id: item.id.videoId,
          title: item.snippet.title,
          description: item.snippet.description,
          thumbnail: item.snippet.thumbnails.high.url,
          author: item.snippet.channelTitle,
          date: new Date(item.snippet.publishedAt).toLocaleDateString(),
          duration: this.formatDuration(
            contentDetailsData.items[index].contentDetails.duration
          ),
        }));
      } catch (err) {
        this.error = `Error al obtener los datos: ${err.message}`;
      } finally {
        this.loading = false;
      }
    },
    formatDuration(duration) {
      const match = duration.match(/PT(\d+H)?(\d+M)?(\d+S)?/);
      const hours = parseInt(match[1]) || 0;
      const minutes = parseInt(match[2]) || 0;
      const seconds = parseInt(match[3]) || 0;

      if (hours > 0) {
        return `${hours}:${minutes.toString().padStart(2, "0")}:${seconds
          .toString()
          .padStart(2, "0")}`;
      } else {
        return `${minutes}:${seconds.toString().padStart(2, "0")}`;
      }
    },
    openVideo(videoId) {
      this.currentVideoId = videoId;
      this.showModal = true;
      document.body.classList.add("overflow-hidden");
    },
    closeModal() {
      this.showModal = false;
      document.body.classList.remove("overflow-hidden");
    },
  },
};
</script>

<style>
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

/* Estilos adicionales para prevenir el scroll */
body.overflow-hidden {
  overflow: hidden;
}
</style>
