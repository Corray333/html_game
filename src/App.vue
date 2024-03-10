<template>
  <div class="console">
    <div class="pad">
      <div class="pad_top">
        <div class="dinamic">
          <div class="sound_dot"></div>
          <div class="sound_dot"></div>
          <div class="sound_dot"></div>
          <div class="sound_dot"></div>
          <div class="sound_dot"></div>
          <div class="sound_dot"></div>
          <div class="sound_dot"></div>
          <div class="sound_dot"></div>
          <div class="sound_dot"></div>
          <div class="sound_dot"></div>
          <div class="sound_dot"></div>
          <div class="sound_dot"></div>
          <div class="sound_dot"></div>
          <div class="sound_dot"></div>
          <div class="sound_dot"></div>
          <div class="sound_dot"></div>
          <div class="sound_dot"></div>
          <div class="sound_dot"></div>
        </div>
      </div>
      <div class="pad_middle">
        <div class="stick" id="left_stick">
          <div class="stick-inside"></div>
        </div>
        <div class="arrows">
          <img src="./assets/arrow.png" alt="" class="arrow" id="up">
          <div class="horizontal_arrows">
            <img src="./assets/arrow.png" alt="" class="arrow" id="left">
            <img src="./assets/arrow.png" alt="" class="arrow" id="right">
          </div>
          <img src="./assets/arrow.png" alt="" class="arrow" id="down">
        </div>
      </div>
      <div class="pad_bottom"></div>
    </div>
    <div class="screen">
      <div class="screen_edge">
        <div class="world">
          <div class="map">
            <img src="./assets/map.png" alt="" class="map_bg">
            <img src="" alt="" class="player">
            <img v-if="world != undefined" v-for="(obj, i) of world.objects" :key="i" :src="obj.image"
              :class="obj.class" :style="`top:calc(12px*${obj.x});left:calc(12px*${obj.y})`" alt="" @click="test">
            <img v-if="world != undefined" v-for="(obj, i) of world.objects" :key="i"
              :style="`position: absolute; border: 2px solid red; top:calc(12px*${obj.collider.y});left:calc(12px*${obj.collider.x}); width:calc(12px*${obj.collider.width}); height:calc(12px*${obj.collider.height});`">
            <div v-if="world != undefined" class="center" :style="`width: 5px; height: 5px; top:calc(1px*${world.player.y});left:calc(1px*${world.player.x})`"></div>
            </div>
        </div>
      </div>
    </div>
    <div class="pad">
      <div class="dinamic">
        <div class="sound_dot"></div>
        <div class="sound_dot"></div>
        <div class="sound_dot"></div>
        <div class="sound_dot"></div>
        <div class="sound_dot"></div>
        <div class="sound_dot"></div>
        <div class="sound_dot"></div>
        <div class="sound_dot"></div>
        <div class="sound_dot"></div>
        <div class="sound_dot"></div>
        <div class="sound_dot"></div>
        <div class="sound_dot"></div>
        <div class="sound_dot"></div>
        <div class="sound_dot"></div>
        <div class="sound_dot"></div>
        <div class="sound_dot"></div>
        <div class="sound_dot"></div>
        <div class="sound_dot"></div>
      </div>
      <div class="buttons">
        <div class="button">X</div>
        <div class="horizontal_buttons">
          <div class="button">Y</div>
          <div class="button">A</div>
        </div>
        <div class="button">B</div>
      </div>
      <div class="stick">
        <div class="stick-inside"></div>
      </div>
    </div>
  </div>

</template>

<script setup>
import { ref, onMounted } from 'vue'

const keys = {
  a: false,
  d: false,
  w: false,
  s: false
}
window.addEventListener('keydown', (e) => {
  if (e.key == "a") {
    keys.a = true
  }
  if (e.key == "d") {
    keys.d = true
  }
  if (e.key == "w") {
    keys.w = true
  }
  if (e.key == "s") {
    keys.s = true
  }
})
window.addEventListener('keyup', (e) => {
  if (e.key == "a") {
    keys.a = false
  }
  if (e.key == "d") {
    keys.d = false
  }
  if (e.key == "w") {
    keys.w = false
  }
  if (e.key == "s") {
    keys.s = false
  }
})

class Props {
  constructor(name, img, x, y, cx, cy, cw, ch) {
    this.x = x
    this.y = y
    this.width = 0
    this.height = 0
    this.image = img
    this.class = name
    this.collider = {
      x: cx,
      y: cy,
      width: cw,
      height: ch
    }
  }
}

const speed = 6
const tileSize = 12

const test = (e) => {
  console.log(e.target.height)
}

class Player {
  constructor() {
    this.height = tileSize * 1.5
    this.width = tileSize * 1
    this.x = -260;
    this.y = 380;
    this.currentAnimation = 0
    this.direction = "down"
    this.img = document.querySelector('.player')
    this.animateInterval = null
  }
  draw() {
    this.img.src = `./src/assets/player_${this.direction}${this.currentAnimation}.png`
    this.img.style.top = `${this.y}px`
    this.img.style.left = `${this.x}px`
  }
  animate() {
    this.animateInterval = setInterval(() => {
      this.currentAnimation = (this.currentAnimation + 1) % 4
    }, 100)
  }

}


class Map {
  constructor() {
    this.image = new Image()
    this.world = document.querySelector('.map')
    this.globalOffsetX = 600
    this.globalOffsetY = 0
    this.scale = 2
    this.objects = [
      new Props('lake', './src/assets/lake.png', 22, 106, 102, 18, 4, 4),
      new Props('house', './src/assets/house_lvl1.png', 23, -24, -24, 26, 4, 4),
    ]
    this.player = new Player()
  }
  get offsetX() {
    return this.globalOffsetX
  }
  get offsetY() {
    return this.globalOffsetY
  }
  set offsetX(value) {
    this.globalOffsetX = value
  }
  set offsetY(value) {
    this.globalOffsetY = value
  }
  animate() {
    switch (true) {
      case keys.a && keys.w:
        this.player.direction = "left"
        if (this.player.animateInterval == null) {
          this.player.animate()
        }
        leftStick.style.transform = "translate(-25px, -25px)"
        this.offsetX += speed
        this.offsetY += speed
        this.player.x -= speed
        this.player.y -= speed

        for (let i = 0; i < this.objects.length; i++) {
          const obj = this.objects[i]
          if (this.player.x / 12 <= obj.collider.x + obj.collider.width && this.player.x / 12 >= obj.collider.x && this.player.y / 12 <= obj.collider.y + obj.collider.height && this.player.y / 12 >= obj.collider.y) {
            console.log(this.player.x / 12, obj.collider.x + obj.collider.width, this.player.x / 12 + this.player.width, obj.collider.x, this.player.y / 12, obj.collider.y + obj.collider.height, this.player.y / 12 + this.player.height, obj.collider.y)
            this.offsetX -= speed
            this.offsetY -= speed
            this.player.x += speed
            this.player.y += speed
          }
        }

        break;
      case keys.a && keys.s:
        this.player.direction = "left"
        if (this.player.animateInterval == null) {
          this.player.animate()
        }
        leftStick.style.transform = "translate(-25px, 25px)"
        this.offsetX += speed
        this.offsetY -= speed
        this.player.x -= speed
        this.player.y += speed
        for (let i = 0; i < this.objects.length; i++) {
          const obj = this.objects[i]
          if (this.player.x / 12 <= obj.collider.x + obj.collider.width && this.player.x / 12 >= obj.collider.x && this.player.y / 12 <= obj.collider.y + obj.collider.height && this.player.y / 12 >= obj.collider.y) {
            console.log(this.player.x / 12, obj.collider.x + obj.collider.width, this.player.x / 12 + this.player.width, obj.collider.x, this.player.y / 12, obj.collider.y + obj.collider.height, this.player.y / 12 + this.player.height, obj.collider.y)
            this.offsetX -= speed
            this.offsetY += speed
            this.player.x += speed
            this.player.y -= speed
          }
        }
        break;
      case keys.d && keys.w:
        this.player.direction = "right"
        if (this.player.animateInterval == null) {
          this.player.animate()
        }
        leftStick.style.transform = "translate(25px, -25px)"
        this.offsetX -= speed
        this.offsetY += speed
        this.player.x += speed
        this.player.y -= speed

        for (let i = 0; i < this.objects.length; i++) {
          const obj = this.objects[i]
          if (this.player.x / 12 <= obj.collider.x + obj.collider.width && this.player.x / 12 >= obj.collider.x && this.player.y / 12 <= obj.collider.y + obj.collider.height && this.player.y / 12 >= obj.collider.y) {
            console.log(this.player.x / 12, obj.collider.x + obj.collider.width, this.player.x / 12 + this.player.width, obj.collider.x, this.player.y / 12, obj.collider.y + obj.collider.height, this.player.y / 12 + this.player.height, obj.collider.y)
            this.offsetX += speed
            this.offsetY -= speed
            this.player.x -= speed
            this.player.y += speed
          }
        }

        break;
      case keys.d && keys.s:
        this.player.direction = "right"
        if (this.player.animateInterval == null) {
          this.player.animate()
        }
        leftStick.style.transform = "translate(25px, 25px)"
        this.offsetX -= speed
        this.offsetY -= speed
        this.player.x += speed
        this.player.y += speed

        for (let i = 0; i < this.objects.length; i++) {
          const obj = this.objects[i]
          if (this.player.x / 12 <= obj.collider.x + obj.collider.width && this.player.x / 12 >= obj.collider.x && this.player.y / 12 <= obj.collider.y + obj.collider.height && this.player.y / 12 >= obj.collider.y) {
            console.log(this.player.x / 12, obj.collider.x + obj.collider.width, this.player.x / 12 + this.player.width, obj.collider.x, this.player.y / 12, obj.collider.y + obj.collider.height, this.player.y / 12 + this.player.height, obj.collider.y)
            this.offsetX += speed
            this.offsetY += speed
            this.player.x -= speed
            this.player.y -= speed
          }
        }

        break;
      case keys.a:
        this.player.direction = "left"
        if (this.player.animateInterval == null) {
          this.player.animate()
        }
        leftStick.style.transform = "translate(-25px, 0)"
        this.offsetX += speed
        this.player.x -= speed

        for (let i = 0; i < this.objects.length; i++) {
          const obj = this.objects[i]
          if (this.player.x / 12 <= obj.collider.x + obj.collider.width && this.player.x / 12 >= obj.collider.x && this.player.y / 12 <= obj.collider.y + obj.collider.height && this.player.y / 12 >= obj.collider.y) {
            console.log(this.player.x / 12, obj.collider.x + obj.collider.width, this.player.x / 12 + this.player.width, obj.collider.x, this.player.y / 12, obj.collider.y + obj.collider.height, this.player.y / 12 + this.player.height, obj.collider.y)
            this.offsetX -= speed
            this.player.x += speed
          }
        }

        break;
      case keys.d:
        this.player.direction = "right"
        if (this.player.animateInterval == null) {
          this.player.animate()
        }
        leftStick.style.transform = "translate(25px, 0)"
        this.offsetX -= speed
        this.player.x += speed

        for (let i = 0; i < this.objects.length; i++) {
          const obj = this.objects[i]
          if (this.player.x / 12 <= obj.collider.x + obj.collider.width && this.player.x / 12 >= obj.collider.x && this.player.y / 12 <= obj.collider.y + obj.collider.height && this.player.y / 12 >= obj.collider.y) {
            console.log(this.player.x / 12, obj.collider.x + obj.collider.width, this.player.x / 12 + this.player.width, obj.collider.x, this.player.y / 12, obj.collider.y + obj.collider.height, this.player.y / 12 + this.player.height, obj.collider.y)
            this.offsetX += speed
            this.player.x -= speed
          }
        }

        break;
      case keys.w:
        this.player.direction = "up"
        if (this.player.animateInterval == null) {
          this.player.animate()
        }
        leftStick.style.transform = "translate(0, -25px)"
        this.offsetY += speed
        this.player.y -= speed

        for (let i = 0; i < this.objects.length; i++) {
          const obj = this.objects[i]
          if (this.player.x / 12 <= obj.collider.x + obj.collider.width && this.player.x / 12 >= obj.collider.x && this.player.y / 12 <= obj.collider.y + obj.collider.height && this.player.y / 12 >= obj.collider.y) {
            console.log(this.player.x / 12, obj.collider.x + obj.collider.width, this.player.x / 12, obj.collider.x, this.player.y / 12, obj.collider.y + obj.collider.height, this.player.y / 12, obj.collider.y)
            this.offsetY -= speed
            this.player.y += speed
          }
        }

        break;
      case keys.s:
        this.player.direction = "down"
        if (this.player.animateInterval == null) {
          this.player.animate()
        }
        leftStick.style.transform = "translate(0, 25px)"
        this.offsetY -= speed
        this.player.y += speed

        for (let i = 0; i < this.objects.length; i++) {
          const obj = this.objects[i]
          if (this.player.x / 12 <= obj.collider.x + obj.collider.width && this.player.x / 12 >= obj.collider.x && this.player.y / 12 <= obj.collider.y + obj.collider.height && this.player.y / 12 >= obj.collider.y) {
            console.log(this.player.x / 12, obj.collider.x + obj.collider.width, this.player.x / 12 + this.player.width, obj.collider.x, this.player.y / 12, obj.collider.y + obj.collider.height, this.player.y / 12 + this.player.height, obj.collider.y)
            this.offsetY += speed
            this.player.y -= speed
          }
        }

        break;
      default:
        clearInterval(this.player.animateInterval)
        leftStick.style.transform = "translate(0, 0)"
        this.player.animateInterval = null
        break;
    }
    this.world.style.transform = `translate(${this.offsetX}px, ${this.offsetY}px) scale(1)`
    this.player.draw()
  }
}

const world = ref()
let leftStick
onMounted(() => {
  world.value = new Map()
  leftStick = document.querySelector('#left_stick')
  setInterval(() => {
    world.value.animate()
  }, 1000 / 60)
})









</script>

<style scoped>
.console {
  display: flex;
  padding: 25px;
  box-shadow: inset 11px -9px 18.5px rgba(0, 0, 0, 0.250), inset -11px 9px 15.2px rgba(255, 255, 255, 0.250);
  background-color: var(--accent-color);
  border-radius: 125px;
}

.pad {
  display: flex;
  flex-direction: column;
  justify-content: space-around;
  align-items: center;
  padding: 0 50px;
}

.pad_middle {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 50px;
}

.dinamic {
  display: grid;
  grid-template-columns: repeat(6, 15px);
  gap: 5px;
}

.sound_dot {
  width: 15px;
  height: 15px;
  background-color: #1b1b1b;
  border-radius: 50%;
  box-shadow: inset 1px -1px 1.2px rgba(255, 255, 255, 0.75);
}

.stick {
  align-self: center;
  justify-self: center;
  display: flex;
  justify-content: center;
  align-items: center;
  width: 100px;
  height: 100px;
  border-radius: 50px;
  background-color: var(--accent-color);
  box-shadow: inset -1px 1px 0.6px rgba(255, 255, 255, 0.250), -1px 1px 7px 4px rgba(0, 0, 0, 0.250);
  transition: all 0.3s;
}

.stick-inside {
  width: 75px;
  height: 75px;
  background-color: rgba(27, 27, 27, 0.2);
  border-radius: 50%;
  box-shadow: inset 1px -1px 1.2px rgba(255, 255, 255, 0.250);
}

.arrows {
  cursor: pointer;
  display: flex;
  flex-direction: column;
  align-items: center;
}

.arrow#up {
  filter: drop-shadow(-1px 1px 2px rgba(0, 0, 0, 0.250)) drop-shadow(1px 1px 2px rgba(0, 0, 0, 0.250));
}

.arrow#left {
  transform: rotate(-90deg);
  filter: drop-shadow(-1px -1px 2px rgba(0, 0, 0, 0.250)) drop-shadow(-1px -1px 2px rgba(0, 0, 0, 0.250));
}

.arrow#down {
  filter: drop-shadow(1px -1px 2px rgba(0, 0, 0, 0.250)) drop-shadow(1px -1px 2px rgba(0, 0, 0, 0.250));
  transform: rotate(180deg);
}

.arrow#right {
  rotate: 90deg;
  filter: drop-shadow(1px 1px 2px rgba(0, 0, 0, 0.250)) drop-shadow(1px 1px 2px rgba(0, 0, 0, 0.250));
}

.horizontal_arrows {
  display: flex;
  /* justify-content: space-between; */
  width: 100%;
  gap: 35px;
  margin-top: -10px;
  margin-bottom: -10px;
}

.arrow {
  width: 40px;
}

.screen_edge {
  padding: 25px;
  background-color: #1b1b1b;
  border-radius: 25px;
}

.buttons {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  gap: 5px
}

.horizontal_buttons {
  display: flex;
  gap: 50px
}

.button {
  cursor: pointer;
  display: flex;
  justify-content: center;
  align-items: center;
  height: 60px;
  width: 60px;
  border-radius: 30px;
  background-color: var(--accent-color);
  box-shadow: inset -1px 1px 0.6px rgba(255, 255, 255, 0.250), -1px 1px 7px 4px rgba(0, 0, 0, 0.250);
}


.world {
  width: 700px;
  height: 700px;
  overflow: hidden;
  border-radius: 15px;
  position: relative;
}

.world * {
  transform: scale(2);
  transform-origin: left top;
  position: absolute;
  image-rendering: pixelated;
}

.map {
  position: relative;
}

.map_bg {
  position: relative;
}

.house {
  position: absolute;
  width: 48px;
  transition: all 0.3s;
}

.house:hover {
  transform: scale(2.2);
}

.player {
  width: 18px;
}
.center{
  height: 25px;
  width: 25px;
  border-radius: 25px;
  background-color: red;
  top: 0;
  left: 0;
}
</style>
