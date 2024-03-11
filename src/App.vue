<template>
  <div class="console">
    <div class="pad">
      <div class="pad_top">
        <!-- <p v-if="world != null">X:{{ Math.floor(world.player.x/12) }}</p> -->
        <!-- <p v-if="world != null">Y:{{ Math.floor(world.player.y/12) }}</p> -->
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
          <div class="money" v-if="world != undefined">
            <img src="./assets/coin.png" alt="">
            <p>{{ world.player.money }}</p>
          </div>
          <div class="intentory">
            <div class="backpack_icon">
              <img src="./assets/backpack.png" alt="">
            </div>
          </div>
          <div class="map">
            <img src="./assets/map.png" alt="" class="map_bg">
            <div class="shadow" v-if="world != undefined"
              :style="`top:${world.player.y}px; left: ${world.player.x}px; background-color: rgba(0,0,0,0.2); width:38px; height:18px; border-radius:10px; transform:scaleY(0.5) scaleX(0.9) translateX(-5px) translateY(65px);`">
            </div>
            <img src="" alt="" class="player">
            <img v-if="world != undefined" v-for="(obj, i) of world.objects" :key="i" :src="obj.image"
              :class="obj.class" :style="`top:calc(12px*${obj.x});left:calc(12px*${obj.y})`" alt="" @click="test">
            <img class="collider" v-if="world != undefined" v-for="(obj, i) of world.objects" :key="i"
              :style="`top:calc(12px*${obj.collider.y});left:calc(12px*${obj.collider.x}); width:calc(12px*${obj.collider.width}); height:calc(12px*${obj.collider.height});`">
            <!-- <div v-if="world != undefined" class="center" :style="`width: 5px; height: 5px; top:calc(1px*${world.player.y});left:calc(1px*${world.player.x})`"></div> -->
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
  if (e.key == "Tab") {
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
  constructor(name, x, y, cx, cy, cw, ch) {
    this.x = x
    this.y = y
    this.width = 0
    this.height = 0
    this.class = name
    this.name = name
    this.collider = {
      x: cx - cw / 2,
      y: cy - ch / 2,
      width: cw * 2,
      height: ch * 2
    }
  }
  get image() {
    return `./src/assets/${this.name}.png`
  }
}

class Upgradable extends Props {
  constructor(name, x, y, cx, cy, cw, ch) {
    super(name, x, y, cx, cy, cw, ch)
    this.level = 0
    this.class += " upgradable"
  }
  get image() {
    return `./src/assets/${this.name}_lvl${this.level}.png`
  }
}

class Bed extends Upgradable {
  constructor(name, x, y, cx, cy, cw, ch) {
    super(name, x, y, cx, cy, cw, ch)
    this.class += " bed"
  }
  get image() {
    return `./src/assets/bed_lvl${this.level}_${this.name}.png`
  }
}

const speed = 4
const tileSize = 12

const test = (e) => {
  console.log(e.target.height)
}

class Player {
  constructor() {
    this.height = tileSize * 1.5
    this.width = tileSize * 1
    this.x = -260;
    this.y = 390;
    this.currentAnimation = 0
    this.direction = "down"
    this.img = document.querySelector('.player')
    this.animateInterval = null
    this.inventory = []
    this.money = 100
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
      // Get from json
      new Props('lake', 17, 102, 102, 17, 4, 5),
      new Upgradable('house', 23, -24, -24, 26, 4, 4),
      new Props('tree', -5, 25, 25.5, -1.5, 2.5, 1.5),
      new Props('tree', -3, 20, 20.5, 0.5, 2.5, 1.5),
      new Props('tree', -9, 30, 30.5, -5.5, 2.5, 1.5),
      new Props('tree', -7, 36, 36.5, -3.5, 2.5, 1.5),
      new Props('tree', -1, 29, 29.5, 1.5, 2.5, 1.5),
      new Props('tree', -9, 27 + 30, 27.5 + 30, -5.5, 2.5, 1.5),
      new Props('tree', -7, 36 + 30, 36.5 + 30, -3.5, 2.5, 1.5),
      new Props('tree', -5, 30 + 30, 30.5 + 30, -1.5, 2.5, 1.5),
      new Props('tree', -3, 25 + 30, 25.5 + 30, 0.5, 2.5, 1.5),
      new Upgradable('apple_tree', 30, -15, -14, 36, 3.5, 1.5),
      new Upgradable('apple_tree', 30, -35, -34, 36, 3.5, 1.5),
      new Bed('wheat', 54, -24, -24, 54, 4, 4),
      new Bed('wheat', 64, -24, -24, 64, 4, 4),
      new Bed('tomatoes', 54, -8, -8, 54, 4, 4),
      new Bed('tomatoes', 64, -8, -8, 64, 4, 4),
      new Bed('tomatoes', 74, -8, -8, 74, 4, 4),
      new Props('edge', 0, 0, 17, 38, 2, 4),
      new Props('edge', 0, 0, 19, 35, 2, 2),
      new Props('edge', 0, 0, 21, 33, 2, 2),
      new Props('edge', 0, 0, 23, 31, 2, 2),
      new Props('edge', 0, 0, 25, 29, 2, 2),
      new Props('edge', 0, 0, 36, 28, 20, 2),
      new Props('edge', 0, 0, 19, 44, 2, 2),
      new Props('edge', 0, 0, 21, 46, 2, 2),
      new Props('edge', 0, 0, 23, 48, 2, 4),
      new Props('edge', 0, 0, 25, 54, 2, 4),
      new Props('edge', 0, 0, 27, 60, 2, 6),
      new Props('edge', 0, 0, 25, 70, 2, 2),
      new Props('edge', 0, 0, 23, 72, 2, 2),
      new Props('edge', 0, 0, 21, 74, 2, 2),
      new Props('edge', 0, 0, 19, 76, 2, 2),
      new Props('edge', 0, 0, 15, 78, 4, 2),
      new Props('edge', 0, 0, 11, 80, 2, 2),
      new Props('edge', 0, 0, -7, 82, 13, 2),
      new Props('edge', 0, 0, -17, 80, 2, 2),
      new Props('edge', 0, 0, -20, 78, 2, 2),
      new Props('edge', 0, 0, -22, 76, 2, 2),
      new Props('edge', 0, 0, -26, 74, 2, 2),
      new Props('edge', 0, 0, -28, 72, 2, 2),
      new Props('edge', 0, 0, -30, 70, 2, 2),
      new Props('edge', 0, 0, -32, 68, 2, 2),
      new Props('edge', 0, 0, -34, 66, 2, 2),
      new Props('edge', 0, 0, -37, 60, 2, 4),
      new Props('edge', 0, 0, -39, 57, 2, 2),
      new Props('edge', 0, 0, -41, 55, 2, 2),
      new Props('edge', 0, 0, -43, 51, 2, 3),
      new Props('edge', 0, 0, -45, 47, 2, 2),
      new Props('edge', 0, 0, -47, 36, 2, 6),
      new Props('edge', 0, 0, -46, 29, 2, 2),
      new Props('edge', 0, 0, -45, 22, 2, 6),
      new Props('edge', 0, 0, -43, 16, 2, 2),
      new Props('edge', 0, 0, -41, 13, 2, 2),
      new Props('edge', 0, 0, -38, 10, 2, 2),
      new Props('edge', 0, 0, -27.5, 55, 1, 10),
      new Props('edge', 0, 0, -0.5, 57, 1, 15),
      new Props('edge', 0, 0, -24, 50, 5, 1),
      new Props('edge', 0, 0, -9, 50, 5, 1),
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
          if (this.player.x / 12 <= obj.collider.x + obj.collider.width && (this.player.x + this.player.width * 2) / 12 >= obj.collider.x && this.player.y / 12 <= obj.collider.y + obj.collider.height && (this.player.y + this.player.height * 2) / 12 >= obj.collider.y) {
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
          if (this.player.x / 12 <= obj.collider.x + obj.collider.width && (this.player.x + this.player.width * 2) / 12 >= obj.collider.x && this.player.y / 12 <= obj.collider.y + obj.collider.height && (this.player.y + this.player.height * 2) / 12 >= obj.collider.y) {
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
          if (this.player.x / 12 <= obj.collider.x + obj.collider.width && (this.player.x + this.player.width * 2) / 12 >= obj.collider.x && this.player.y / 12 <= obj.collider.y + obj.collider.height && (this.player.y + this.player.height * 2) / 12 >= obj.collider.y) {
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
          if (this.player.x / 12 <= obj.collider.x + obj.collider.width && (this.player.x + this.player.width * 2) / 12 >= obj.collider.x && this.player.y / 12 <= obj.collider.y + obj.collider.height && (this.player.y + this.player.height * 2) / 12 >= obj.collider.y) {
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
          if (this.player.x / 12 <= obj.collider.x + obj.collider.width && (this.player.x + this.player.width * 2) / 12 >= obj.collider.x && this.player.y / 12 <= obj.collider.y + obj.collider.height && (this.player.y + this.player.height * 2) / 12 >= obj.collider.y) {
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
          if (this.player.x / 12 <= obj.collider.x + obj.collider.width && (this.player.x + this.player.width * 2) / 12 >= obj.collider.x && this.player.y / 12 <= obj.collider.y + obj.collider.height && (this.player.y + this.player.height * 2) / 12 >= obj.collider.y) {
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
          if (this.player.x / 12 <= obj.collider.x + obj.collider.width && (this.player.x + this.player.width * 2) / 12 >= obj.collider.x && this.player.y / 12 <= obj.collider.y + obj.collider.height && (this.player.y + this.player.height * 2) / 12 >= obj.collider.y) {
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
          if (this.player.x / 12 <= obj.collider.x + obj.collider.width && (this.player.x + this.player.width * 2) / 12 >= obj.collider.x && this.player.y / 12 <= obj.collider.y + obj.collider.height && (this.player.y + this.player.height * 2) / 12 >= obj.collider.y) {
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
.collider {
  transform: scale(1) !important;
  position: absolute;
  border: 2px solid red;
  visibility: hidden;
}

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
  gap: 45px;
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
  gap: 60px
}

.button {
  cursor: pointer;
  display: flex;
  justify-content: center;
  align-items: center;
  height: 60px;
  width: 60px;
  border-radius: 30px;
  font-weight: bold;
  font-size: 2em;
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
  position: absolute;
  image-rendering: pixelated;
}

.map {
  position: relative;
}

.map_bg {
  position: relative;
  left: -86px;
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
  width: 15px;
  transform-origin: left top;
}

.center {
  height: 25px;
  width: 25px;
  border-radius: 25px;
  background-color: red;
  top: 0;
  left: 0;
}

.apple_tree {
  width: 60px;
}

.upgradable {
  transition: all 0.3s;
}

.upgradable:hover {
  transform: scale(2.1);
}

.bed:hover {
  transform: scale(2.2);
}

.money {
  transform: none;
  position: absolute;
  top: 0;
  right: 0;
  padding: 25px;
  color: white;
  z-index: 100;
  font-size: 12px;
  background-color: #efca88;
  border: 5px solid #b89a67;
  border-radius: 15px;
  display: flex;
  justify-content: center;
  align-items: center;
}
.money>*{
  position: relative;
  gap:25px;
}

.screen_edge {
  position: relative;
}
</style>
