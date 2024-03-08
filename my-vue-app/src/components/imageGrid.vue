<template>
  <div id="wrapper" ref="wrapper" @mousemove="handleMouseMove">
      <div id="background"></div>
      <div id="element1" ref="element1"></div>
      <div id="element2" ref="element2"></div>
      <div id="element3" ref="element3"></div>
      <div id="element4" ref="element4"></div>
      <div id="element5" ref="element5"></div>
      <div id="element6" ref="element6"></div>
      <div id="element7" ref="element7"></div>
  </div>
</template>

<script>
import { ref, onMounted, onBeforeUnmount } from 'vue';
import gsap from 'gsap';

export default {
  setup() {
    const wrapper = ref(null);
    const element1 = ref(null);
    const element2 = ref(null);
    const element3 = ref(null);
    const element4 = ref(null);
    const element5 = ref(null);
    const element6 = ref(null);
    const element7 = ref(null);

    onMounted(() => {
      // GSAP setup for initial positions
      const elements = [element1, element2, element3, element4, element5, element6, element7];
      const timeline = gsap.timeline();

      elements.forEach((element, index) => {
        const { width, height, backgroundColor, top, left, right } = getInitialStyles(index + 1);
        gsap.set(element.value, {
          width: 0,
          height: 0,
          backgroundColor,
          top,
          left,
          right,
          scale: 0.2,
          transformOrigin: '50% 50%',
          opacity: 0,
        });

        timeline.to(element.value, {
          width,
          height,
          opacity: 1,
          scale: 1,
          duration: 0.5,
          ease: 'power2.out',
        });
      });
    });

    const handleMouseMove = (e) => {
      const mouseX = e.pageX;
      const mouseY = e.pageY;

      const elements = [element1, element2, element3, element4, element5, element6, element7];
      elements.forEach((element, index) => {
        parallax(element.value, mouseX, mouseY, 12 + 2 * index);
      });
    };

    const parallax = (element, mouseX, mouseY, speed) => {
      if (element) {
        gsap.to(element, {
          x: -(mouseX - window.innerWidth / 2) / speed,
          y: -(mouseY - window.innerHeight / 2) / speed,
          ease: 'power2.out',
        });
      }
    };

    const getInitialStyles = (index) => {
      switch (index) {
        case 1:
          return { width: '10%', height: '200px', backgroundColor: 'green', top: '10%', left: '60px' };
        case 2:
          return { width: '15%', height: '300px', backgroundColor: 'blue', top: '20%', right: '0' };
        case 3:
          return { width: '20%', height: '200px', backgroundColor: 'rgb(0, 242, 255)', top: '40px', right: '750px' };
        case 4:
          return { width: '20%', height: '250px', backgroundColor: 'rgb(255, 0, 195)', top: '20px', right: '370px' };
        case 5:
          return { width: '30%', height: '150px', backgroundColor: 'rgb(255, 0, 225)', top: '320px', right: '540px' };
        case 6:
          return { width: '15%', height: '300px', backgroundColor: 'rgb(0, 255, 72)', top: '300px', right: '250px' };
        case 7:
          return { width: '20%', height: '300px', backgroundColor: 'rgb(221, 255, 0)', top: '300px', left: '0px' };
        default:
          return {};
      }
    };

    onBeforeUnmount(() => {
      // Cleanup logic if needed
    });

    return { handleMouseMove, wrapper, element1, element2, element3, element4, element5, element6, element7 };
  },
};
</script>

<style scoped>
#wrapper {
  margin: 0;
  height: 100vh;
}

body {
  overflow: hidden;
}

#element1 {
  position: absolute;
  top: 10%;
  left: 60px;
  width: 10%;
  background-color: green;
  height: 200px;
  border: 1px solid black;
}

#element2 {
  position: absolute;
  top: 20%;
  right: 0;
  width: 15%;
  background-color: blue;
  height: 300px;
  border: 1px solid black;
}

#element3 {
  position: absolute;
  right: 750px;
  top: 40px;
  width: 20%;
  background-color: rgb(0, 242, 255);
  height: 200px;
  border: 1px solid black;
}

#element4 {
  position: absolute;
  right: 370px;
  top: 20px;
  width: 20%;
  background-color: rgb(255, 0, 195);
  height: 250px;
  border: 1px solid black;
}

#element5 {
  position: absolute;
  right: 540px;
  top: 320px;
  width: 30%;
  background-color: rgb(255, 0, 225);
  height: 150px;
  border: 1px solid black;
}

#element6 {
  position: absolute;
  right: 250px;
  top: 300px;
  width: 15%;
  background-color: rgb(0, 255, 72);
  height: 300px;
  border: 1px solid black;
}

#element7 {
  position: absolute;
  left: 0px;
  top: 300px;
  width: 20%;
  background-color: rgb(221, 255, 0);
  height: 300px;
  border: 1px solid black;
}

#background {
  background-color: red;
  width: 100%;
  height: 600px;
}
</style>
