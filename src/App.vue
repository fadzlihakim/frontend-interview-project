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
          image: "assets/product-image-front-large.jpeg",
          thumbnail: "assets/product-image-front-thumbnail.jpeg",
        },
        {
          tag: "image-large",
          image: "assets/product-image-large.jpeg",
          thumbnail: "assets/product-image-thumbnail.jpeg",
        },
        {
          tag: "back-large",
          image: "assets/product-image-back-large.jpeg",
          thumbnail: "assets/product-image-back-thumbnail.jpeg",
        },
      ],
      videos: [
        { tag: "double-tap", video: "videos/double-tap-zoom-mobile.mp4" },
        { tag: "drag-swipe", video: "videos/drag-swipe-in-mobile.mp4" },
        { tag: "single-click", video: "videos/single-click-zoom-desktop.mp4" },
      ],
      isHovered: false,
      currentIndex: 0,
      translateValue: 0,
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
  },
};
</script>

<template>
  <header class="grid place-items-center">
    <div class="container">
      <img class="headerImage" src="/assets/tbps-logo.webp" alt="" />
    </div>
  </header>
  <section class="grid place-items-center">
    <div class="container" id="content">
      <div class="grid md:grid-cols-2 gap-10 p-5 grid-cols-1">
        <div class="bg-white p-5">
          <div class="slider relative overflow-hidden">
            <div
              class="relative place-items-center flex flex-row overflow-hidde transition-transform duration-500 ease-in-out"
              :style="{ transform: 'translateX(' + translateValue + ')' }">
              <div
                v-for="(item, index) in image"
                :key="index"
                class="slider-item">
                <img :src="item.image" alt="Slider Image" />
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
          <div class="flex flex-row overflow-hidden">
            <!-- <div class="thumbnail"> -->
            <img
              class="thumbnailImage"
              v-for="item in image"
              v-bind:src="item.thumbnail" />
            <video
              class="thumbnailVideo"
              v-for="item in videos"
              v-bind:src="item.video"></video>
          </div>
        </div>
        <div class="flex flex-col text-start" id="describe">
          <label
            ><span class="uppercase">{{ brand }}</span>
            <span class="status">{{ status }}</span></label
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
          <div class="flex justify-center md:justify-start py-10">
            <button class="counterButton" @click="counter--">-</button>
            <label id="counter">{{ counter }}</label>
            <button class="counterButton" @click="counter++">+</button>
          </div>
          <button type="button" class="button">Add to Basket</button>
        </div>
      </div>
    </div>
  </section>
</template>
