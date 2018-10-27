<template>
  <div class="wave" id="wave">
    <vue-p5 @setup="setup" @draw="draw"></vue-p5>
  </div>
</template>
<script>
import VueP5 from 'vue-p5';

export default {
  name: 'Wave',
  components: {
    "vue-p5": VueP5
  },
  data() {
    return {
      width: 0,
      height: 0,
      cols: 0,
      rows: 0,
      scl: 40,
      terrain: [],
      flying: 0
    }
  },
  methods: {
    setup(sk) {
      this.width = document.getElementById("wave").clientWidth
      this.height = document.getElementById("wave").clientHeight
      // using resizeCanvas because Canvas were created in Ant.vue file
      sk.createCanvas(this.width, this.height, sk.WEBGL);
      if (this.width <= 768) {
        this.scl = 80
      } else {
        this.scl = 60
      }
      this.cols = sk.floor(this.width / this.scl) * 4
      this.rows = sk.floor(this.height / this.scl) * 4
    },
    draw(sk) {
      let width = document.getElementById("wave").clientWidth
      let height = document.getElementById("wave").clientHeight
      let absWidth = Math.abs(this.width - width)
      let absHeight = Math.abs(this.height - height)
      if (absWidth > 100 || absHeight > 100) {
        this.width = document.getElementById("wave").clientWidth
        this.height = document.getElementById("wave").clientHeight
        sk.resizeCanvas(this.width, this.height, sk.WEBGL);
        if (this.width <= 768) {
          this.scl = 80
        } else {
          this.scl = 60
        }
        this.cols = sk.floor(this.width / this.scl) * 4
        this.rows = sk.floor(this.height / this.scl) * 4
      }


      // Create 2d array
      this.flying -= 0.1
      this.terrain = new Array(this.cols);
      let xoff = this.flying
      for (let x = 0; x < this.cols; x++) {
        this.terrain[x] = new Array(this.rows);
        let yoff = 0
        for (let y = 0; y < this.rows; y++) {
          this.terrain[x][y] = sk.map(sk.noise(xoff, yoff), 0, 1, -120, 120)
          yoff += 0.2
        }
        xoff += 0.2
      }



      sk.background(0)
      sk.stroke(255)
      sk.noFill()

      sk.rotateX(Math.PI / 3)
      sk.rotateZ(Math.PI / 2)
      sk.frameRate(10)
      sk.translate(-this.width / 4, -this.height / 0.5, -this.height / 2)

      for (let y = 0; y < this.rows; y++) {
        sk.beginShape(sk.TRIANGLE_STRIP)
        for (let x = 0; x < this.cols; x++) {
          sk.vertex(x * this.scl, y * this.scl, this.terrain[x][y])
          sk.vertex(x * this.scl, (y + 1) * this.scl, this.terrain[x][y + 1])
        }
        sk.endShape()
        sk.beginShape(sk.LINES)
        for (let x = 0; x < this.cols; x++) {
          sk.vertex(x * this.scl, y * this.scl, this.terrain[x][y])
        }
        sk.endShape()
      }
    }
  }
}
</script>

<style scoped>
.wave {
  width: 100%;
  height: 100%;
  position: absolute;
  left: 0;
  top: 0;
  z-index: -1;
}
</style>
