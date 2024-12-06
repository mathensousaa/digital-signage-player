<script setup>
import CymaDisplayPlayer from '@/modules/cymadisplay/components/cymadisplay-player.vue'

const mediaList = [
  {
    descricao: 'Imagem 1',
    src: '/src/assets/media/4a1bb95d5d504537a1b825e6d36bf4f9_spaten-3.jpg',
    type: 'image',
  },
  {
    descricao: 'Vídeo 1',
    src: '/src/assets/media/ab9af8495f3246b2b19220c5513521d5_Video-Patagonia-High-Quality.mp4',
    type: 'video',
  },
  {
    descricao: 'Imagem 2',
    src: '/src/assets/media/dc299db0e80e401db8cd009ce3771801_Spaten-2.mp4',
    type: 'video',
  },
];

</script>

<template>
  <CymaDisplayPlayer :medias="mediaList" />
</template>