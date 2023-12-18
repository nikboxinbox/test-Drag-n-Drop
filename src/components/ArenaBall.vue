<template>
  <div class="wrapp">
    <div
      class="arena"
      ref="arena"
      @mousedown="startDrag"
      @mousemove="drag"
      @mouseup="stopDrag"
    >
      <table class="arena-grid">
        <tr v-for="row in 4" :key="row">
          <td v-for="col in 4" :key="col"></td>
        </tr>
      </table>

      <div
        class="ball"
        :class="{ active: dragging }"
        :style="{ top: ballY + 'px', left: ballX + 'px' }"
        ref="ball"
      ></div>
    </div>

    <div class="coordinates">
      <p>X: {{ normalizedX.toFixed(2) }}</p>

      <p>Y: {{ normalizedY.toFixed(2) }}</p>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      dragging: false,
      ballX: 300,
      ballY: 300,
      mouseX: 0,
      mouseY: 0,
      arenaRect: null,
      normalizedX: 0,
      normalizedY: 0,
    };
  },

  methods: {
    startDrag(event) {
      this.dragging = true;
      this.mouseX = event.clientX;
      this.mouseY = event.clientY;
      this.arenaRect = this.$refs.arena.getBoundingClientRect();

      this.normalizedCoordinates();
      document.addEventListener("mouseup", this.onMouseUpOutside);
    },

    drag(event) {
      if (this.dragging) {
        let deltaX = event.clientX - this.mouseX;
        let deltaY = event.clientY - this.mouseY;
        this.ballX += deltaX;
        this.ballY += deltaY;
        this.mouseX = event.clientX;
        this.mouseY = event.clientY;

        this.checkBoundaries();
        this.normalizedCoordinates();
      }
    },

    stopDrag() {
      this.dragging = false;
      document.removeEventListener("mouseup", this.onMouseUpOutside);
    },

    onMouseUpOutside(event) {
      if (!this.$refs.arena.contains(event.target)) {
        this.stopDrag();
      }
    },

    checkBoundaries() {
      if (this.ballX < 0) this.ballX = 0;
      if (this.ballX > this.arenaRect.width) this.ballX = this.arenaRect.width;
      if (this.ballY < 0) this.ballY = 0;
      if (this.ballY > this.arenaRect.height)
        this.ballY = this.arenaRect.height;
    },

    normalizedCoordinates() {
      this.normalizedX = (this.ballX / this.arenaRect.width) * 2 - 1;
      this.normalizedY = -(this.ballY / this.arenaRect.height) * 2 + 1;
    },
  },
};
</script>

<style scoped>
.wrapp {
  width: 100%;
  height: 100%;
  display: flex;
  align-items: center;
  justify-content: center;
}

.arena {
  width: 600px;
  aspect-ratio: 1 / 1;
  border: 2px solid rgb(231, 5, 209);
  border-radius: 8px;
  padding: 10px;
  position: relative;

  &::after {
    content: "";
    width: 10px;
    height: 10px;
    background-color: rgb(231, 5, 209);
    border-radius: 50%;
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    z-index: 1;
  }
}

.arena-grid {
  width: 100%;
  height: 100%;
  position: absolute;
  top: 0;
  left: 0;
  border-spacing: 0;
  z-index: -1;

  td {
    border-left: 1px solid #eee;
    border-bottom: 1px solid #eee;
  }
}

.ball {
  width: 20px;
  height: 20px;
  background-color: rgb(13, 220, 243);
  border-radius: 50%;
  position: absolute;
  z-index: 2;
}
.active {
  box-shadow: 0 0 12px rgba(245, 70, 207, 0.8);
}
.coordinates {
  position: absolute;
  bottom: 13%;
}
</style>
