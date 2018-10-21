<template>
  <div class="sketch" id="sketch">
    <code>
      ant: {{ant}}
      columns: {{columns}}
      rows: {{rows}}
    </code>
    <vue-p5 @setup="setup" @draw="draw">
    </vue-p5>
  </div>
</template>

<script>
import VueP5 from 'vue-p5';

export default {
  name: 'Ant',
  components: {
    "vue-p5": VueP5
  }, data() {
    return {
      width: 0,
      height: 0,
      w: 0,
      columns: 0,
      rows: 0,
      board: [],
      ant: {
        x: 50,
        y: 50,
        dir: 0
      }
    }
  },
  methods: {
    // Setup canvas
    setup(sketch) {
      this.width = document.getElementById("sketch").clientWidth
      this.height = document.getElementById("sketch").clientHeight
      sketch.createCanvas(this.width, this.height);
      this.w = 20;

      this.init(sketch);
    },
    // Update every frame
    draw(sketch) {
      if (this.width !== document.getElementById("sketch").clientWidth || this.height !== document.getElementById("sketch").clientHeight) {
        this.width = document.getElementById("sketch").clientWidth
        this.height = document.getElementById("sketch").clientHeight
        sketch.createCanvas(this.width, this.height);
        this.init(sketch);
      }

      sketch.background(255);
      // set round for skipping frame
      let round = 1
      for (let i = 0; i < round; i++) {
        this.walk(sketch)
      }
      this.render(sketch)
    },
    walk(sketch) {
      if (this.ant.x > this.columns - 1) {
        this.ant.x = 0
      } else if (this.ant.x < 0) {
        this.ant.x = this.columns - 1
      }

      if (this.ant.y > this.rows - 1) {
        this.ant.y = 0
      } else if (this.ant.y < 0) {
        this.ant.y = this.rows - 1
      }

      this.rotate(sketch)
      this.moveForward(sketch);
    },
    init(sketch) {
      // Calculate columns and rows
      this.columns = sketch.floor(this.width / this.w);
      this.rows = sketch.floor(this.height / this.w);
      // Initial ant position
      this.ant.x = sketch.floor(this.columns / 2)
      this.ant.y = sketch.floor(this.rows / 2)
      // Create 2d array
      this.board = new Array(this.columns);
      for (let i = 0; i < this.columns; i++) {
        this.board[i] = new Array(this.rows);
        for (let j = 0; j < this.rows; j++) {
          this.board[i][j] = 0
        }
      }
    },
    // Rotate ant
    rotate(sketch) {
      let tile = this.board[this.ant.x][this.ant.y]
      if (tile === 0) {
        this.board[this.ant.x][this.ant.y] = 1
        this.ant.dir++;
      } else {
        this.board[this.ant.x][this.ant.y] = 0
        this.ant.dir--;
      }
    },
    // Move ant forward for 1 point
    moveForward(sketch) {
      this.ant.dir = (this.ant.dir + 4) % 4

      switch (this.ant.dir) {
        case 0:
          this.ant.y--;
          break;
        case 1:
          this.ant.x++;
          break;
        case 2:
          this.ant.y++;
          break;
        case 3:
          this.ant.x--;
          break;
      }
    },
    // Render array
    render(sketch) {
      for (let i = 0; i < this.columns; i++) {
        for (let j = 0; j < this.rows; j++) {
          if ((this.board[i][j] == 1)) sketch.fill(0);
          else sketch.fill(255);
          sketch.stroke(255);
          sketch.rect(i * this.w, j * this.w, this.w - 1, this.w - 1);
        }
      }
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.sketch {
  width: 100%;
  height: 100%;
  position: absolute;
  left: 0;
  top: 0;
  z-index: -1;
}
</style>
