<!-- Please remove this file from your project -->
<template>
  <div
    class="
      relative
      items-top
      mx-auto
      min-h-screen
      sm:items-center sm:pt-0
    "
  >
    <div class="flex justify-center my-8">
      <input type="button" class="bg-indigo-500 px-4 text-white py-1 cursor-pointer rounded-md" value="Clear" @click="clearCanvas()" />
    </div>
    <div class="flex justify-center">
      <canvas
        ref="myCanvas"
        width="900"
        height="600"
        style="border: 1px solid #d3d3d3"
      ></canvas>
    </div>
  </div>
</template>

<script>
export default {
  name: 'NuxtTutorial',
  data() {
    return {
      canvas: '',
      context: '',
      posini: '',
      pointsClick: [],
    }
  },
  mounted() {
    this.canvas = this.$refs.myCanvas
    this.context = this.canvas.getContext('2d')
    this.canvas.addEventListener('click', this.DrawClick, false)
  },
  methods: {
    DrawClick(e) {
      const pos = this.getMousePos(this.canvas, e)

      if (this.posini && this.pointsClick.length <= 4) {
        if (this.pointsClick.length < 4) {
          this.context.fillRect(pos.x, pos.y, 5, 5)
        }

        this.context.beginPath()
        this.context.strokeStyle = '#000'
        this.context.moveTo(this.posini.x, this.posini.y)
        this.context.lineTo(pos.x, pos.y)
        this.context.stroke()
      } else {
        this.posini = ''
        this.pointsClick = []
        this.context.fillRect(pos.x, pos.y, 5, 5)
      }
      this.posini = pos
      this.pointsClick.push(this.posini)
    },
    clearCanvas() {
      this.posini = null
      this.pointsClick = []
      this.context.clearRect(0, 0, this.canvas.width, this.canvas.height)
    },

    getMousePos(canvas, e) {
      const rect = canvas.getBoundingClientRect()
      return {
        x: e.clientX - rect.left,
        y: e.clientY - rect.top,
      }
    },
  },
}
</script>


