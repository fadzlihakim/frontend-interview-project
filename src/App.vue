<script>
export default {
  setup() {
    const onSwiper = (swiper) => {
      // console.log(swiper);
    };
    const onSlideChange = () => {
      // console.log("slide change");
    };
    return {
      onSwiper,
      onSlideChange,
      modules: [Navigation],
    };
  },
  data() {
    return {
      counter: 0,
      status: "Hurry Limited Stock",
      brand: "Samsung",
      description: "Samsung Galaxy S23 Plus 5G",
      price: 880.0,
      sim: "Dual Sim",
      condition: "Like New",
      colour: "Lavender",
      network: "Unlocked",
      storage: "512GB",
      image: [
        {
          tag: "front-large",
          image: "product-image-front-large.jpeg",
          thumbnail: "product-image-front-thumbnail.jpeg",
        },
        {
          tag: "image-large",
          image: "product-image-large.jpeg",
          thumbnail: "product-image-thumbnail.jpeg",
        },
        {
          tag: "back-large",
          image: "product-image-back-large.jpeg",
          thumbnail: "product-image-back-thumbnail.jpeg",
        },
      ],
      videos: [
        {
          tag: "double-tap",
          video: "double-tap-zoom-mobile.mp4",
        },
        {
          tag: "drag-swipe",
          video: "drag-swipe-in-mobile.mp4",
        },
        {
          tag: "single-click",
          video: "single-click-zoom-desktop.mp4",
        },
      ],
      isHovered: false,
      currentIndex: 0,
      translateValue: 0,
      touchStartX: 0,
      touchEndX: 0,
      threshold: 50,
      isModalOpen: false,
      modalContent: "",
      modalContentType: "",
    };
  },
  methods: {
    moveEnlargedView(event) {
      // Update the position of the enlarged view based on the cursor
      if (this.isHovered) {
        const enlargedView = this.$refs.enlargedView;
        const rect = enlargedView.getBoundingClientRect();

        const x = event.clientX - rect.width / 2;
        const y = event.clientY - rect.height / 2;

        enlargedView.style.left = `${x}px`;
        enlargedView.style.top = `${y}px`;
      }
    },
    nextSlide() {
      this.currentIndex = (this.currentIndex + 1) % this.image.length;
      this.updateSlider();
    },
    prevSlide() {
      this.currentIndex =
        (this.currentIndex - 1 + this.image.length) % this.image.length;
      this.updateSlider();
    },
    updateSlider() {
      this.translateValue = `-${this.currentIndex * 100}%`;
    },
    handleTouchStart(event) {
      this.touchStartX = event.changedTouches[0].clientX;
    },
    handleTouchMove(event) {
      this.touchEndX = event.changedTouches[0].clientX;
    },
    handleTouchEnd() {
      const deltaX = this.touchEndX - this.touchStartX;

      if (Math.abs(deltaX) > this.threshold) {
        if (deltaX > 0) {
          this.prevSlide();
        } else {
          this.nextSlide();
        }
      }
    },
    openModal(content, contentType) {
      this.modalContent = content;
      this.modalContentType = contentType;
      this.isModalOpen = true;
    },
    closeModal() {
      this.isModalOpen = false;
    },
  },
};
</script>

<template>
  <header class="grid place-items-center">
    <div class="container">
      <img class="headerImage" src="/src/assets/tbps-logo.webp" alt="" />
    </div>
  </header>
  <section class="grid place-items-center">
    <div class="container" id="content">
      <div class="grid md:grid-cols-2 gap-10 p-5 grid-cols-1">
        <div class="bg-white p-5">
          <div class="relative overflow-hidden h-96">
            <div
              @touchstart="handleTouchStart"
              @touchmove="handleTouchMove"
              @touchend="handleTouchEnd"
              class="relative place-items-center flex flex-row overflow-hidden transition-transform duration-500 ease-in-out"
              :style="{ transform: 'translateX(' + translateValue + ')' }">
              <div
                v-for="(item, index) in image"
                :key="index"
                class="grid place-items-center flex-[0_0_100%]">
                <img
                  class="h-96"
                  :src="`/src/assets/${item.image}`"
                  alt="Slider Image" />
              </div>
            </div>

            <div
              class="arrow start-10 cursor-pointer absolute top-1/2 -ml-8 text-3xl text-gray-800"
              @click="prevSlide">
              &#9665;
            </div>
            <div
              class="arrow end-10 cursor-pointer absolute top-1/2 -mr-8 text-3xl text-gray-800"
              @click="nextSlide">
              &#9655;
            </div>
          </div>
          <div class="flex flex-row overflow-hidden pt-5">
            <img
              class="thumbnailImage"
              v-for="item in image"
              :src="`/src/assets/${item.thumbnail}`"
              @click="openModal(item, 'image')" />
            <video
              class="thumbnailVideo"
              v-for="item in videos"
              :src="`/src/assets/videos/${item.video}`"
              @click="openModal(item.video, 'video')"></video>
          </div>
        </div>
        <!-- Modal -->
        <div
          v-if="isModalOpen"
          class="fixed inset-0 flex items-center justify-center">
          <div class="modal-overlay fixed inset-0 bg-black opacity-25"></div>

          <div
            class="modal-container bg-white w-1/2 p-6 rounded shadow-lg grid place-items-center">
            <div
              v-if="modalContentType === 'image'"
              class="grid place-items-center flex-[0_0_100%]">
              <img
                :src="`/src/assets/${modalContent.image}`"
                alt=""
                class="h-96" />
            </div>
            <div
              v-if="modalContentType === 'video'"
              class="grid place-items-center flex-[0_0_100%]">
              <iframe
                :src="`/src/assets/videos/${modalContent}`"
                class="h-96"></iframe>
            </div>
            <div class="mt-4">
              <button
                @click="closeModal"
                class="bg-gray-500 hover:bg-gray-700 text-white font-bold py-2 px-4 rounded">
                Close
              </button>
            </div>
          </div>
        </div>
        <div class="flex flex-col text-start" id="describe">
          <label
            ><span class="uppercase">{{ brand }}</span>
            <span class="float-right text-green">{{ status }}</span></label
          >
          <h2 class="text-2xl font-bold">{{ description }}</h2>
          <h3 class="text-xl font-bold">&#163;{{ price }}</h3>
          <hr />
          <label class="py-2"
            >Sim: <span class="uppercase">{{ sim }}</span></label
          >
          <label class="py-2"
            >Condition: <span class="uppercase">{{ condition }}</span></label
          >
          <label class="py-2"
            >Colour: <span class="uppercase">{{ colour }}</span></label
          >
          <label class="py-2"
            >Network: <span class="uppercase">{{ network }}</span></label
          >
          <label class="py-2"
            >Storage: <span class="uppercase">{{ storage }}</span>
          </label>
          <hr />
          <div class="flex justify-center md:justify-start py-5">
            <button class="text-xl border-none" @click="counter--">-</button>
            <label id="counter">{{ counter }}</label>
            <button class="text-xl border-none" @click="counter++">+</button>
          </div>
          <button type="button" class="button">Add to Basket</button>
        </div>
      </div>
    </div>
  </section>
</template>
<style scoped>
.modal-overlay {
  position: absolute;
  width: 100%;
  height: 100%;
  z-index: 10;
}

.modal-container {
  max-height: 80vh;
  overflow-y: auto;
  z-index: 20;
}

.modal-close {
  font-size: 1.5rem;
}

@media screen and (max-width: 768px) {
  .modal-container {
    height: 60vh;
    width: 90vw;
  }
}
</style>
