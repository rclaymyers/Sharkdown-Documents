<script setup lang="ts">
import { ref } from "vue";
import { type Gallery } from "../../../sharedModels/Gallery";
import { UtilitiesService } from "../services/utils";
import LightboxWrapper from "./LightboxWrapper.vue";

const props = defineProps<{ gallery: Gallery }>();
const showGalleria = ref<boolean>(false);
let activeIndex = 1;

const openGalleria = (index: number): void => {
  activeIndex = index;
  showGalleria.value = true;
};
</script>

<template>
  <div class="gallery-image-list" data-cy="document-image-gallery">
    <div class="gallery-image" v-for="(imagePath, index) in gallery.imagePaths">
      <img
        :src="UtilitiesService.prependApiDomain(imagePath)"
        class="gallery-item-contents clickable"
        @click="openGalleria(index)"
        data-cy="lightbox-trigger"
      />
    </div>
  </div>
  <LightboxWrapper
    :initialActiveIndex="activeIndex"
    :gallery="props.gallery"
    v-if="showGalleria"
    @gallery-closed="showGalleria = false"
  ></LightboxWrapper>
</template>

<style>
.galleria-thumbnail {
  max-height: 5vh;
  max-width: 5vh;
}
</style>
