
<template>
    <div
      ref="container"
      class="parallax-container"
      @mousemove="handleMouseMove"
      @mouseleave="handleMouseLeave"
      @mouseenter="handleMouseEnter"
    >
      <div v-for="(block, index) in blocks" :key="index" :class="`imageContainer blocco_${index + 1}`">
        <img :src="`../assets/img/logo.png`" alt="" class="image" />
      </div>
    </div>
  </template>
  
  <script>
  import { ref, onMounted, onBeforeUnmount } from 'vue';
  import gsap from 'gsap';
  
  export default {
    setup() {
      const blocks = ref([
        { width: 400, height: 250, backgroundColor: 'blueviolet', top: '0px', left: '10px' },
        { width: 180, height: 350, backgroundColor: 'red', right: '580px', top: '40px' },
        { width: 190, height: 170, backgroundColor: 'blue', right: '100px', top: '20px' },
        { width: 200, height: 150, backgroundColor: 'pink', right: '320px', top: '320px' },
        { width: 180, height: 280, backgroundColor: 'green', right: '50px', top: '400px' },
        { width: 260, height: 200, backgroundColor: 'orange', left: '260px', top: '300px' },
        { width: 150, height: 400, backgroundColor: 'burlywood', left: '-50px', top: '300px' },
        { width: 400, height: 100, backgroundColor: 'yellow', left: '500px', top: '600px' },
      ]);
  
      const container = ref(null);
  
      let mouseX = 0;
      let mouseY = 0;
      let isMouseOverPage = true;
  
      const handleMouseMove = (event) => {
        if (isMouseOverPage) {
          mouseX = (event.clientX / window.innerWidth) * 2 - 1;
          mouseY = -((event.clientY - window.innerHeight / 2) / window.innerHeight) * 2 + 1;
  
          const sensitivityFactor = 2;
          const maxTranslationX = 200;
          const maxTranslationY = 200;
  
          const newX = -mouseX * maxTranslationX * sensitivityFactor;
          const newY = mouseY * maxTranslationY * sensitivityFactor;
  
          gsap.to(container.value, {
            x: newX,
            y: newY,
            ease: 'power3.inOut',
          });
        }
      };
  
      const handleMouseLeave = () => {
        isMouseOverPage = false;
        
        // Reimposta la trasformazione del container
        gsap.to(container.value, {
          x: 0,
          y: 0,
          ease: 'power3.inOut',
        });
      };
  
      const handleMouseEnter = () => {
        isMouseOverPage = true;
      };
  
      onMounted(() => {
        gsap.registerPlugin();
  
        blocks.value.forEach((block, index) => {
          const { width, height, backgroundColor, top, left, right } = block;
          const blockElement = container.value.querySelector(`.blocco_${index + 1}`);
  
          gsap.set(blockElement, {
            width,
            height,
            backgroundColor,
            top,
            left,
            right,
            scale: 0.2,
            transformOrigin: '50% 50%',
            opacity: 0,
          });
  
          gsap.to(blockElement, {
            width,
            height,
            opacity: 1,
            scale: 1,
            duration: 0.5,
            delay: 0.2 * index,
          });
        });
      });
  
      onBeforeUnmount(() => {
        // Cleanup logic if needed
      });
  
      return { blocks, handleMouseMove, handleMouseLeave, handleMouseEnter, container };
    },
  };
  </script>
  
  <style scoped>
  body {
    margin: 0;
    overflow: hidden;
  }
  
  .parallax-container {
    position: relative;
    height: 100vh;
    width: 100%;
    border: solid 1px red;
    display: flex;
    justify-content: center;
    align-items: center;
    overflow: hidden;
  }
  
  .imageContainer {
    position: absolute;
    overflow: hidden;
    transition: transform 0.3s ease-in-out;
  }
  
  .image {
    width: 100%;
    height: 100%;
    object-fit: contain;
  }
  
  .imageContainer:hover {
    transform: scale(1.2);
  }
  </style>
  