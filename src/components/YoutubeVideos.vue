<template>
  <div class="container mx-auto px-2 py-8 lg:px-32">
    <div class="flex justify-between items-center mb-2">
      <h2 class="text-3xl font-bold dark:text-white">Servicios</h2>
    </div>
    <div
      v-if="error"
      class="flex items-start sm:items-center bg-red-100 border border-red-400 text-red-700 px-4 py-3 rounded"
      role="alert"
    >
      <svg
        class="w-5 h-5 sm:w-6 sm:h-6 mt-1 sm:mt-0 mr-2 flex-shrink-0"
        xmlns="http://www.w3.org/2000/svg"
        fill="none"
        viewBox="0 0 24 24"
        stroke="currentColor"
      >
        <path
          stroke-linecap="round"
          stroke-linejoin="round"
          stroke-width="2"
          d="M12 8v4m0 4h.01M21 12a9 9 0 11-18 0 9 9 0 0118 0z"
        />
      </svg>
      <span class="text-sm sm:text-base">{{ error }}</span>
    </div>
    <!-- Estado de carga -->
    <div
      v-else-if="loading"
      class="flex flex-col justify-center items-center h-64"
    >
      <div class="loader mb-4"></div>
      <p class="text-gray-700 dark:text-gray-300">Cargando videos...</p>
    </div>
    <div v-else>
      <!-- Selector de listas de reproducción -->
      <div class="mb-4 flex flex-col sm:flex-row sm:items-center">
        <label
          for="playlist-select"
          class="mb-2 sm:mb-0 sm:mr-2 text-lg font-medium"
        >
          Filtrar por lista:
        </label>
        <div class="relative w-full sm:w-auto">
          <select
            id="playlist-select"
            v-model="selectedPlaylist"
            @change="fetchVideos"
            class="w-full sm:w-auto appearance-none bg-white border border-gray-300 rounded-md py-3 px-4 pr-8 text-lg leading-tight focus:outline-none focus:border-blue-500 focus:ring focus:ring-blue-200"
          >
            <option :value="null">Todos los videos</option>
            <option
              v-for="playlist in playlists"
              :key="playlist.id"
              :value="playlist.id"
            >
              {{ playlist.title }}
            </option>
          </select>
          <div
            class="pointer-events-none absolute inset-y-0 right-0 flex items-center px-2 text-gray-700"
          >
            <svg
              class="fill-current h-4 w-4"
              xmlns="http://www.w3.org/2000/svg"
              viewBox="0 0 20 20"
            >
              <path
                d="M9.293 12.95l.707.707L15.657 8l-1.414-1.414L10 10.828 5.757 6.586 4.343 8z"
              />
            </svg>
          </div>
        </div>
      </div>

      <!-- Video destacado -->
      <div v-if="videos.length > 0" class="mb-4">
        <div
          class="flex flex-col md:flex-row bg-white rounded-lg overflow-hidden cursor-pointer relative"
          @click="openVideo(videos[0].id)"
        >
          <div class="relative">
            <img
              :src="videos[0].thumbnail"
              :alt="videos[0].title"
              class="w-full h-[193px] md:w-[690px] md:h-[390px] object-cover rounded-lg"
            />
            <div
              class="absolute bottom-2 right-2 bg-black bg-opacity-75 text-white px-2 py-1 rounded-md text-sm"
            >
              {{ videos[0].duration }}
            </div>
            <div
              v-if="videos[0].isShort"
              class="absolute top-2 right-2 bg-red-500 text-white px-2 py-1 rounded-md text-xs font-bold"
            >
              Short
            </div>
            <div
              class="absolute top-2 left-2 bg-blue-500 text-white px-2 py-1 rounded-md text-xs font-bold"
            >
              Más reciente
            </div>
          </div>
          <div
            class="md:flex md:flex-col md:justify-evenly md:w-1/2 pt-2 sm:p-6"
          >
            <h2 class="hidden md:block text-lg sm:text-2xl font-bold mb-2">
              {{ videos[0].title }}
            </h2>
            <p class="hidden md:block text-gray-600 mb-2">
              {{ videos[0].description }}
            </p>
            <p class="text-sm text-gray-500">{{ videos[0].date }}</p>
          </div>
        </div>
      </div>

      <!-- Mensaje cuando no hay videos -->
      <div v-else-if="!loading" class="text-center text-gray-600 my-8">
        No hay videos en esta lista de reproducción.
      </div>

      <!-- Carrusel de videos -->
      <h3 v-if="videos.length > 1" class="text-xl font-bold mb-4">
        Últimos videos
      </h3>
      <swiper
        v-if="videos.length > 1"
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
            class="bg-white rounded-lg shadow-md overflow-hidden mb-12 h-[300px] sm:h-[365px] xl:h-[400px] cursor-pointer relative"
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
              <div
                v-if="video.isShort"
                class="absolute top-2 right-2 bg-red-500 text-white px-2 py-1 rounded-md text-xs font-bold"
              >
                Short
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

const CACHE_DURATION = 24 * 60 * 60 * 1000; // 24 horas en milisegundos

function isLocalStorageAvailable() {
  try {
    const testKey = "__test__";
    localStorage.setItem(testKey, testKey);
    localStorage.removeItem(testKey);
    return true;
  } catch (e) {
    return false;
  }
}

const storage =
  typeof window !== "undefined" && isLocalStorageAvailable()
    ? localStorage
    : {
        getItem: () => null,
        setItem: () => {},
      };

function getCachedData(key) {
  const cachedData = storage.getItem(key);
  if (cachedData) {
    try {
      const { data, timestamp } = JSON.parse(cachedData);
      return { data, timestamp };
    } catch (e) {
      console.error("Error parsing cached data:", e);
    }
  }
  return null;
}

function setCachedData(key, data) {
  const cacheObject = {
    data: data,
    timestamp: Date.now(),
  };
  try {
    storage.setItem(key, JSON.stringify(cacheObject));
  } catch (e) {
    console.error("Error setting cached data:", e);
  }
}

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
      playlists: [],
      selectedPlaylist: null,
    };
  },
  mounted() {
    this.fetchPlaylists();
    this.fetchVideos();
  },
  methods: {
    async fetchPlaylists() {
      const cachedData = getCachedData("playlists");
      if (cachedData) {
        this.playlists = cachedData.data;
        if (Date.now() - cachedData.timestamp < CACHE_DURATION) {
          return;
        }
      }

      try {
        const response = await fetch(
          `https://www.googleapis.com/youtube/v3/playlists?part=snippet&channelId=${this.channelId}&key=${this.apiKey}&maxResults=10`
        );
        const data = await response.json();
        this.playlists = data.items.map((item) => ({
          id: item.id,
          title: item.snippet.title,
        }));
        setCachedData("playlists", this.playlists);
      } catch (error) {
        console.error("Error fetching playlists:", error);
      }
    },

    async fetchVideos(useCache = true) {
      const cacheKey = this.selectedPlaylist
        ? `videos_${this.selectedPlaylist}`
        : "videos_all";

      if (useCache) {
        const cachedData = getCachedData(cacheKey);
        if (cachedData) {
          this.videos = cachedData.data;
          this.loading = false;

          if (Date.now() - cachedData.timestamp < CACHE_DURATION) {
            // Si los datos en caché son recientes, los usamos y actualizamos en segundo plano
            this.fetchVideos(false);
            return;
          }
        }
      }

      try {
        this.loading = true;
        let url;
        if (this.selectedPlaylist) {
          url = `https://www.googleapis.com/youtube/v3/playlistItems?part=snippet&playlistId=${this.selectedPlaylist}&key=${this.apiKey}&maxResults=7`;
        } else {
          url = `https://www.googleapis.com/youtube/v3/search?key=${this.apiKey}&channelId=${this.channelId}&part=snippet&type=video&order=date&maxResults=7`;
        }
        const response = await fetch(url);
        const searchData = await response.json();
        if (searchData.error) {
          throw new Error(searchData.error.message);
        }

        const videoIds = searchData.items
          .map((item) =>
            this.selectedPlaylist
              ? item.snippet.resourceId.videoId
              : item.id.videoId
          )
          .join(",");

        const contentDetailsResponse = await fetch(
          `https://www.googleapis.com/youtube/v3/videos?key=${this.apiKey}&id=${videoIds}&part=contentDetails`
        );
        const contentDetailsData = await contentDetailsResponse.json();

        this.videos = searchData.items.map((item, index) => {
          const durationInfo = this.formatDuration(
            contentDetailsData.items[index].contentDetails.duration
          );
          return {
            id: this.selectedPlaylist
              ? item.snippet.resourceId.videoId
              : item.id.videoId,
            title: item.snippet.title,
            description: item.snippet.description,
            thumbnail: item.snippet.thumbnails.high.url,
            author: item.snippet.channelTitle,
            date: new Date(item.snippet.publishedAt).toLocaleDateString(),
            duration: durationInfo.formatted,
            isShort: durationInfo.totalSeconds < 120,
          };
        });

        setCachedData(cacheKey, this.videos);
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

      const totalSeconds = hours * 3600 + minutes * 60 + seconds;

      if (hours > 0) {
        return {
          formatted: `${hours}:${minutes.toString().padStart(2, "0")}:${seconds
            .toString()
            .padStart(2, "0")}`,
          totalSeconds: totalSeconds,
        };
      } else {
        return {
          formatted: `${minutes}:${seconds.toString().padStart(2, "0")}`,
          totalSeconds: totalSeconds,
        };
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

    handlePlaylistChange() {
      this.fetchVideos();
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
