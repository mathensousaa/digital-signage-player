<script setup lang="ts">
import { ref, onMounted, watch } from 'vue';

// Tipos básicos para mídias
type Media = {
  descricao: string; // Uma descrição para identificar a mídia
  src: string;       // Caminho para a mídia
  type: 'image' | 'video'; // Tipo da mídia (imagem ou vídeo)
};

// Props recebendo um array de mídias
const props = defineProps<{
  medias: Media[]; // Array de mídias
}>();

const currentIndex = ref(0); // Índice atual da mídia exibida
const showMedia = ref(true); // Controle para exibir/ocultar a mídia durante a transição

// Referência para o elemento de vídeo
const videoRef = ref<HTMLVideoElement | null>(null);

// Função para alternar para a próxima mídia
function nextMedia() {
  showMedia.value = false; // Inicia a transição de saída

  currentIndex.value = (currentIndex.value + 1) % props.medias.length; // Vai para a próxima mídia
  showMedia.value = true; // Mostra a próxima mídia
}

// Monitora quando o índice atual muda
watch(
  () => currentIndex.value,
  () => {
    const currentMedia = props.medias[currentIndex.value];
    if (currentMedia.type === 'image') {
      setTimeout(nextMedia, 5000);
    }
  },
  { immediate: true }
);

// Inicia o ciclo automático para a primeira mídia (se for imagem)
onMounted(() => {
  if (props.medias[currentIndex.value].type === 'image') {
    setTimeout(nextMedia, 5000);
  }
});
</script>

<template>
  <div class="bg-black h-full w-full relative">
    <div v-if="medias && medias.length > 0" class="w-full h-full overflow-hidden relative">
      <Transition
        name="slide"
        enter-active-class="slide-in-right"
        leave-active-class="slide-out-left"
      >
        <div :key="medias[currentIndex].descricao" v-if="showMedia" class="w-full h-full">
          <!-- Renderiza o vídeo -->
          <video
            v-if="medias[currentIndex].type === 'video'"
            ref="videoRef"
            :src="medias[currentIndex].src"
            class="w-full h-full object-contain"
            autoplay
            muted
            @ended="nextMedia"
          />
          <!-- Renderiza a imagem -->
          <img
            v-else-if="medias[currentIndex].type === 'image'"
            :src="medias[currentIndex].src"
            class="w-full h-full object-contain"
          />
        </div>
      </Transition>
    </div>
  </div>
</template>

<style>
 .slide-in-right {
    -webkit-animation: slide-in-right 0.5s cubic-bezier(0.785, 0.135, 0.15, 0.86) both;
    animation: slide-in-right 0.5s cubic-bezier(0.785, 0.135, 0.15, 0.86) both;
  }

  @keyframes slide-in-right {
    0% {
      -webkit-transform: translateX(1000px);
      transform: translateX(1000px);
      opacity: 0;
    }
    100% {
      -webkit-transform: translateX(0);
      transform: translateX(0);
      opacity: 1;
    }
  }

  .slide-out-left {
    -webkit-animation: slide-out-left 0.5s cubic-bezier(0.55, 0.085, 0.68, 0.53) both;
    animation: slide-out-left 0.5s cubic-bezier(0.55, 0.085, 0.68, 0.53) both;
  }

  @keyframes slide-out-left {
    0% {
      -webkit-transform: translateX(0);
      transform: translateX(0);
      opacity: 1;
    }
    100% {
      -webkit-transform: translateX(-1000px);
      transform: translateX(-1000px);
      opacity: 0;
    }
  }
</style>
