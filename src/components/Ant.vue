<template>
  <div class="sketch" id="sketch">
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
      },
    }
  },
  methods: {
    // Setup canvas
    setup(sk) {
      this.width = document.getElementById("sketch").clientWidth
      this.height = document.getElementById("sketch").clientHeight
      sk.createCanvas(this.width, this.height);
      this.w = 20;

      this.init(sk);
    },
    // Update every frame
    draw(sk) {
      let width = document.getElementById("sketch").clientWidth
      let height = document.getElementById("sketch").clientHeight
      let absWidth = Math.abs(this.width - width)
      let absHeight = Math.abs(this.height - height)
      if (absWidth > 100 || absHeight > 100) {
        this.width = document.getElementById("sketch").clientWidth
        this.height = document.getElementById("sketch").clientHeight
        sk.resizeCanvas(this.width, this.height);
        this.init(sk);
      }

      sk.background(255);
      // set round for skipping frame
      let round = 1
      for (let i = 0; i < round; i++) {
        this.walk()
      }
      this.render(sk)
    },
    walk() {
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

      this.rotate()
      this.moveForward();
    },
    init(sk) {
      // Calculate columns and rows
      this.columns = sk.floor(this.width / this.w);
      this.rows = sk.floor(this.height / this.w);
      // Initial ant position
      this.ant.x = sk.floor(sk.random(this.columns))
      this.ant.y = sk.floor(sk.random(this.rows))
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
    rotate() {
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
    moveForward() {
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
    render(sk) {
      for (let i = 0; i < this.columns; i++) {
        for (let j = 0; j < this.rows; j++) {
          if ((this.board[i][j] == 1)) sk.fill(0);
          else sk.fill(255);
          sk.stroke(255);
          sk.rect(i * this.w, j * this.w, this.w - 1, this.w - 1);
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
