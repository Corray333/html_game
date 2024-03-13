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
            <p>{{ player.money }}</p>
            <img src="./assets/coin.png" alt="">
          </div>
          <transition>
            <div v-if="alertText" class="alert_container">
              <div class="alert_box">
                <p>{{ alertText }}</p>
              </div>
            </div>
          </transition>
          <div class="shop" :class="shopOpened ? '' : 'shop_hidden'">
            <div class="shop_header">
              <button @click="shopSection = true" :class="shopSection ? 'active' : ''">Купить</button>
              <button @click="shopSection = false" :class="shopSection ? '' : 'active'">Продать</button>
              <button @click="closeStore">X</button>
            </div>
            <div class="shop_inside">
              <div class="shop_items">
                <div class="item" v-for="(item, i) of (shopSection ? shopSell : shopBuy)" :key="i"
                  @click="trade(item.name)">
                  <div class="item_image">
                    <img :src="`./src/assets/${item.name}.png`" alt="">
                    <p>{{ item.price }}</p>
                  </div>
                  <p>{{ item.title }}</p>
                </div>
              </div>
              <img src="./assets/trader_portrait.png" alt="" class="trader_portrait">
            </div>
          </div>
          <div class="inventory" :class="backpackOpened ? '' : 'inventory_hidden'" v-if="world != undefined">
            <div class="backpack_icon" @click="changeBackpack">
              <img src="./assets/backpack2.png" alt="">
            </div>
            <div class="backpack_container">
              <div class="item" v-for="(item, i) of player.inventory" :key="i" @click="pickedItem = item.name"
                :class="item.name == pickedItem ? 'picked_item' : ''">
                <div class="item_image">
                  <img :src="`./src/assets/${item.name}.png`" alt="">
                  <p>{{ item.quantity }}</p>
                </div>
                <p>{{ item.title }}</p>
              </div>
            </div>
          </div>
          <div class="map" v-if="world != undefined"
            :style="`transform: translate(${world.offsetX}px, ${world.offsetY}px) scale(1)`">
            <img src="./assets/map.png" alt="" class="map_bg">
            <div class="shadow"
              :style="`top:${player.y}px; left: ${player.x}px; background-color: rgba(0,0,0,0.2); width:38px; height:18px; border-radius:10px; transform:scaleY(0.5) scaleX(0.9) translateX(-5px) translateY(55px);`">
            </div>
            <img :src="player.src" alt="" class="player" :style="`top:${player.y}px; left:${player.x}px`">
            <div class="props" v-for="(obj, i) of world.objects" :key="i" :class="obj.class"
              :style="`top:calc(12px*${obj.x});left:calc(12px*${obj.y})`" @click="world.click(obj.id)">
              <h3 v-if="obj.constructor.price && obj.level == 0">{{ obj.constructor.price }}</h3>
              <img :src="obj.image" alt="">
            </div>
            <img class="collider" v-for="(obj, i) of world.objects" :key="i"
              :style="`top:calc(12px*${obj.collider.y});left:calc(12px*${obj.collider.x}); width:calc(12px*${obj.collider.width}); height:calc(12px*${obj.collider.height});`"
              :id="obj?.id" @click="obj?.id != undefined ? world.click(obj.id) : pass">
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
        <div class="button" @click="playMusic">X</div>
        <div class="horizontal_buttons">
          <div class="button">Y</div>
          <div class="button">A</div>
        </div>
        <div class="button" id="button_b" @click="showAlert('Вау, а ты любознательный😑')">B</div>
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
  if (e.key == "a" || e.key == "ArrowLeft") {
    keys.a = true
  } else if (e.key == "d" || e.key == "ArrowRight") {
    keys.d = true
  } else if (e.key == "w" || e.key == "ArrowUp") {
    keys.w = true
  } else if (e.key == "s" || e.key == "ArrowDown") {
    keys.s = true
  } else if (e.key == "i" || e.key == "I") {
    changeBackpack()
  }
})

const changeBackpack = () => {
  backpackOpened.value = !backpackOpened.value
  if (backpackOpened.value) pickedItem.value = ""
  openSound.play()
  document.querySelector('.arrow#down').style.filter = 'none'
  setTimeout(() => {
    document.querySelector('.arrow#down').style.filter = 'drop-shadow(1px -1px 2px rgba(0, 0, 0, 0.250)) drop-shadow(1px -1px 2px rgba(0, 0, 0, 0.250))'
  }, 100)
}


window.addEventListener('keyup', (e) => {
  if (e.key == "a" || e.key == "ArrowLeft") {
    keys.a = false
  }
  if (e.key == "d" || e.key == "ArrowRight") {
    keys.d = false
  }
  if (e.key == "w" || e.key == "ArrowUp") {
    keys.w = false
  }
  if (e.key == "s" || e.key == "ArrowDown") {
    keys.s = false
  }
})

const speed = 4
const tileSize = 12
const pickedItem = ref("")
const backpackOpened = ref(false)
const shopOpened = ref(false)


const world = ref()
const player = ref()
let leftStick



const music = new Audio('./src/assets/sounds/music.mp3')
const ambient = new Audio('./src/assets/sounds/ambient.mp3')
music.loop = true
music.volume = 0.5
ambient.loop = true


let musicOn = false
const playMusic = () => {
  if (musicOn) {
    music.pause()
    ambient.pause()
    musicOn = false
  } else {
    music.play()
    ambient.play()
    musicOn = true
  }
}

const walkingSound = new Audio('./src/assets/sounds/walking.mp3')
walkingSound.volume = 0.4

const clickSound = new Audio('./src/assets/sounds/click.mp3')
const openSound = new Audio('./src/assets/sounds/open.mp3')



let alertText = ref("")

const closeStore = () => {
  clickSound.play()
  openSound.play()
  shopOpened.value = false
}

const plantsGrowthSpeed = {
  'item_tomatoes_seeds': 15000,
  'item_wheat_seeds': 10000,
  'item_apples': 20000,
  'item_berries': 10000
}

const itemsTitles = {
  'item_tomatoes': 'Томаты',
  'item_wheat': 'Пшеница',
  'item_apples': 'Яблоки',
  'item_tomatoes_seeds': 'Семена томатов',
  'item_wheat_seeds': 'Семена пшеницы',
  'item_fertilizer': 'Удобрение',
  'item_hoe': 'Мотыга',
  'item_watering_can': 'Лейка',
  'item_berries': 'Ягоды'
}


class Item {
  constructor(name, price) {
    this.name = name
    this.price = price
    this.title = itemsTitles[name]
  }
}

const save = () => {
  localStorage.setItem('player', JSON.stringify(player.value))
  world.value.objects.forEach(el => {
    if (el.id) {
      localStorage.setItem(`id${el.id}`, JSON.stringify(el))
    }
  })
}

const load = () => {
  if (localStorage.getItem('player')) {
    let obj = JSON.parse(localStorage.getItem('player'))
    player.value.money = obj.money
    player.value.inventory = obj.inventory
  }
  world.value.objects.forEach(el => {
    if (el.id) {
      let obj = JSON.parse(localStorage.getItem(`id${el.id}`))
      el.level = obj.level
      el.plantingTime = Date.parse(obj.plantingTime)
    }
  })

}

const shopSection = ref(true) // true = sell, false = buy

const shopSell = ref([
  new Item('item_wheat_seeds', 10),
  new Item('item_tomatoes_seeds', 15),
  new Item('item_fertilizer', 15),
  new Item('item_hoe', 15),
  new Item('item_watering_can', 50),
])

const shopBuy = ref([
  new Item('item_wheat', 20),
  new Item('item_tomatoes', 25),
  new Item('item_apples', 35),
  new Item('item_berries', 10),
])

const trade = (item) => {
  if (!shopSection.value) {
    let f = true
    player.value.inventory.forEach((el, i) => {
      if (el.name == item) {
        if (el.quantity == 1) {
          player.value.inventory.splice(i, 1)
        } else {
          el.quantity--
        }
        let price
        shopBuy.value.forEach(el => {
          if (el.name == item) price = el.price
        })
        player.value.money += price
        let audio = new Audio('./src/assets/sounds/buy.wav')
        audio.play()
        f = false
        return
      }
    })
    if (f) showAlert('У меня нет этого товара...')
    return
  } else {
    let price
    shopSell.value.forEach(el => {
      if (el.name == item) price = el.price
    })
    if (player.value.money < price) {
      showAlert('Мне не хватит денег...')
      return
    }
    player.value.money -= price
    player.value.addToInventory(item)
    let audio = new Audio('./src/assets/sounds/buy.wav')
    audio.play()
  }
}

let alertInterval
let alertAudio
const showAlert = (text) => {
  if (alertAudio) alertAudio.pause()
  alertAudio = new Audio('./src/assets/sounds/talking.mp3')
  alertAudio.play()
  let i = 0
  alertText.value = ""
  if (alertInterval) clearInterval(alertInterval)
  alertInterval = setInterval(() => {
    alertText.value += text[i++]
    if (alertText.value == text) {
      alertAudio.pause()
      clearInterval(alertInterval)
      setTimeout(() => {
        alertText.value = ""
        return
      }, 1000)
    }
  }, 50)
}



class Props {
  static elements = 1
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

class Animated extends Props {
  constructor(name, x, y) {
    super(name, x, y, -1000, -1000, 0, 0)
    this.currentAnimation = 0
    this.animateInterval = null
    this.maxAnimation = 2
    this.animationTime = 100
  }
  animate() {
    this.animateInterval = setInterval(() => {
      this.currentAnimation = (this.currentAnimation + 1) % this.maxAnimation
    }, this.animationTime)
  }
}

class Trader extends Animated {
  constructor(name, x, y) {
    super(name, x, y)
    this.id = Props.elements++
    this.class += " trader"
    this.maxAnimation = 2
    this.animationTime = 500
  }
  get image() {
    return `./src/assets/${this.name}_a${this.currentAnimation}.png`
  }
  click() {
    shopOpened.value = true
  }
}

class Upgradable extends Props {
  constructor(name, x, y, cx, cy, cw, ch) {
    super(name, x, y, cx, cy, cw, ch)
    this.level = 0
    this.class += " upgradable"
    this.id = Props.elements++
  }
  get image() {
    return `./src/assets/${this.name}_lvl${this.level}.png`
  }

}

class AppleTree extends Upgradable {
  static price = 50
  constructor(name, x, y, cx, cy, cw, ch) {
    super(name, x, y, cx, cy, cw, ch)
    this.class += " apple_tree"
    this.type = "item_apples"
    this.plantingTime = null
    this.maxLevel = 3
  }
  get image() {
    return `./src/assets/apple_tree_lvl${this.level}.png`
  }
  click() {
    let audio
    switch (this.level) {
      case 0:
        if (player.value.money < AppleTree.price) {
          showAlert('Мне не хватит денег...')
          return
        }
        this.level++
        player.value.money -= AppleTree.price
        audio = new Audio('./src/assets/sounds/buy.wav')
        audio.play()
        AppleTree.price = Math.floor(AppleTree.price * 1.25)
        break
      case 1:
        if (pickedItem.value.substring(5) != 'fertilizer') {
          showAlert('Необходимо удобрение...')
          return
        }
        audio = new Audio('./src/assets/sounds/plow.ogg')
        audio.play()
        this.level++
        player.value.inventory.forEach((item, i) => {
          if (item.name == pickedItem.value) {
            if (item.quantity == 1) {
              player.value.inventory.splice(i, 1)
            } else {
              item.quantity--
            }
          }
        })
        this.plantingTime = new Date()
        break
      case 2:
        showAlert('Слишком рано собирать урожай!')
        return
      case 3:
        audio = new Audio('./src/assets/sounds/collect.mp3')
        audio.play()
        player.value.addToInventory(this.type)
        this.level = 1
        this.plantingTime = null
    }
  }
}

class Berries extends Upgradable {
  static price = 25
  constructor(name, x, y, cx, cy, cw, ch) {
    super(name, x, y, cx, cy, cw, ch)
    this.class += " berry_bush"
    this.type = "item_berries"
    this.plantingTime = null
    this.maxLevel = 3
  }
  get image() {
    return `./src/assets/berries_lvl${this.level}.png`
  }
  click() {
    let audio
    switch (this.level) {
      case 0:
        if (player.value.money < Berries.price) {
          showAlert('Мне не хватит денег...')
          return
        }
        this.level++
        player.value.money -= Berries.price
        audio = new Audio('./src/assets/sounds/buy.wav')
        audio.play()
        Berries.price = Math.floor(Berries.price * 1.25)
        break
      case 1:
        if (pickedItem.value.substring(5) != 'watering_can') {
          showAlert('Необходима лейка...')
          return
        }
        audio = new Audio('./src/assets/sounds/plow.ogg')
        audio.play()
        this.level++
        this.plantingTime = new Date()
        break
      case 2:
        showAlert('Слишком рано собирать урожай!')
        return
      case 3:
        audio = new Audio('./src/assets/sounds/collect.mp3')
        audio.play()
        player.value.addToInventory(this.type)
        this.level = 1
        this.plantingTime = null
    }
  }

}

class Bed extends Upgradable {
  static price = 50
  constructor(name, x, y, cx, cy, cw, ch) {
    super(name, x, y, cx, cy, cw, ch)
    this.class += " bed"
    this.type = ""
    this.plantingTime = null
    this.maxLevel = 6
  }
  get image() {
    return `./src/assets/bed_lvl${this.level}_${this.type}.png`
  }
  click() {
    let audio
    switch (this.level) {
      case 0:
        if (player.value.money < Bed.price) {
          showAlert('Мне не хватит денег...')
          return
        }
        this.level++
        player.value.money -= Bed.price
        audio = new Audio('./src/assets/sounds/buy.wav')
        audio.play()
        Bed.price = Math.floor(Bed.price * 1.25)
        break
      case 1:
        if (pickedItem.value != 'item_hoe') {
          showAlert('Необходима мотыга...')
          return
        }
        this.level++
        audio = new Audio('./src/assets/sounds/plow.ogg')
        audio.play()
        break;
      case 2:
        if (pickedItem.value.substring(pickedItem.value.length - 5, pickedItem.value.length) != 'seeds') {
          showAlert('Надо выбрать семена...')
          return
        }
        this.level++
        audio = new Audio('./src/assets/sounds/plow.ogg')
        audio.play()
        this.type = pickedItem.value
        player.value.inventory.forEach((item, i) => {
          if (item.name == pickedItem.value) {
            if (item.quantity == 1) {
              player.value.inventory.splice(i, 1)
            } else {
              item.quantity--
            }
          }
        })
        this.plantingTime = new Date()
        break
      case 3:
        showAlert('Слишком рано собирать урожай!')
        return
      case 4:
        showAlert('Слишком рано собирать урожай!')
        return
      case 5:
        showAlert('Слишком рано собирать урожай!')
        return
      case 6:
        audio = new Audio('./src/assets/sounds/collect.mp3')
        audio.play()
        player.value.addToInventory(this.type.substring(0, this.type.length - 6))
        this.level = 1
        this.type = ""
        this.plantingTime = null
    }
  }
}




class Player {
  constructor() {
    this.height = tileSize * 1.5
    this.width = tileSize * 1
    this.x = -260;
    this.y = 390;
    this.currentAnimation = 0
    this.direction = "down"
    this.src = './src/assets/player_down1.png'
    this.animateInterval = null
    this.inventory = [
      {
        title: 'Семена пшеницы',
        name: 'item_wheat_seeds',
        quantity: 1
      },
    ]
    this.money = 100
  }
  draw() {
    this.src = `./src/assets/player_${this.direction}${this.currentAnimation}.png`
  }
  animate() {
    this.animateInterval = setInterval(() => {
      this.currentAnimation = (this.currentAnimation + 1) % 4
      walkingSound.play()
    }, 100)
  }

  addToInventory(name) {
    let f = true
    this.inventory.forEach(item => {
      if (item.name == name) {
        item.quantity++
        f = false
        return
      }
    })
    if (f) {
      this.inventory.push({
        name: name,
        title: itemsTitles[name],
        quantity: 1
      })
    }
  }

}


class Map {
  constructor() {
    this.image = new Image()
    // this.world = document.querySelector('.map')
    this.globalOffsetX = 600
    this.globalOffsetY = 0
    this.scale = 2
    this.objects = [
      // Get from json
      new Props('lake', 17, 102, 102, 17, 4, 5),
      new Props('house', 23, -24, -24, 26, 4, 4),
      new Props('tree', -5, 25, 25.5, -1.5, 2.5, 1.5),
      new Props('tree', -3, 20, 20.5, 0.5, 2.5, 1.5),
      new Props('tree', -9, 30, 30.5, -5.5, 2.5, 1.5),
      new Props('tree', -7, 36, 36.5, -3.5, 2.5, 1.5),
      new Props('tree', -1, 29, 29.5, 1.5, 2.5, 1.5),
      new Props('tree', -9, 27 + 30, 27.5 + 30, -5.5, 2.5, 1.5),
      new Props('tree', -7, 36 + 30, 36.5 + 30, -3.5, 2.5, 1.5),
      new Props('tree', -5, 30 + 30, 30.5 + 30, -1.5, 2.5, 1.5),
      new Props('tree', -3, 25 + 30, 25.5 + 30, 0.5, 2.5, 1.5),
      new Props('stone', 15, 55, 55, 16, 1, 0),
      new Props('stone', 23, 30, 30, 23, 1, 0),
      new Props('log', 1, 15, 15, 2, 1, 0),
      new Props('store', -10, 45, 45, -5, 5, 3),
      new AppleTree('apple_tree', 30, -15, -14, 37, 3.5, 1.5),
      new AppleTree('apple_tree', 30, -35, -34, 37, 3.5, 1.5),
      new Berries('berry_bush', 10, -10, -10, 10, 1, 1),
      new Berries('berry_bush', 10, -15, -15, 10, 1, 1),
      new Berries('berry_bush', 10, -20, -20, 10, 1, 1),
      new Berries('berry_bush', 15, -18, -18, 15, 1, 1),
      new Berries('berry_bush', 15, -13, -13, 15, 1, 1),
      new Trader('trader', -2, 45),
      new Bed('bed', 54, -24, -24, 54, 4, 4, 50),
      new Bed('bed', 64, -24, -24, 64, 4, 4, 50),
      new Bed('bed', 54, -8, -8, 54, 4, 4, 50),
      new Bed('bed', 64, -8, -8, 64, 4, 4, 50),
      new Bed('bed', 74, -8, -8, 74, 4, 4, 50),
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
      new Props('edge', 0, 0, -24, 49.5, 5, 1),
      new Props('edge', 0, 0, -9, 49.5, 5, 1),
    ]
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
        player.value.direction = "left"
        if (player.value.animateInterval == null) {
          player.value.animate()
        }
        leftStick.style.transform = "translate(-25px, -25px)"
        this.offsetX += speed / 1.5
        this.offsetY += speed / 1.5
        player.value.x -= speed / 1.5
        player.value.y -= speed / 1.5

        for (let i = 0; i < this.objects.length; i++) {
          const obj = this.objects[i]
          if (player.value.x / 12 <= obj.collider.x + obj.collider.width && (player.value.x + player.value.width * 2) / 12 >= obj.collider.x && player.value.y / 12 <= obj.collider.y + obj.collider.height && (player.value.y + player.value.height * 2) / 12 >= obj.collider.y) {
            this.offsetX -= speed / 1.5
            this.offsetY -= speed / 1.5
            player.value.x += speed / 1.5
            player.value.y += speed / 1.5
          }
        }

        break;
      case keys.a && keys.s:
        player.value.direction = "left"
        if (player.value.animateInterval == null) {
          player.value.animate()
        }
        leftStick.style.transform = "translate(-25px, 25px)"
        this.offsetX += speed / 1.5
        this.offsetY -= speed / 1.5
        player.value.x -= speed / 1.5
        player.value.y += speed / 1.5
        for (let i = 0; i < this.objects.length; i++) {
          const obj = this.objects[i]
          if (player.value.x / 12 <= obj.collider.x + obj.collider.width && (player.value.x + player.value.width * 2) / 12 >= obj.collider.x && player.value.y / 12 <= obj.collider.y + obj.collider.height && (player.value.y + player.value.height * 2) / 12 >= obj.collider.y) {
            this.offsetX -= speed / 1.5
            this.offsetY += speed / 1.5
            player.value.x += speed / 1.5
            player.value.y -= speed / 1.5
          }
        }
        break;
      case keys.d && keys.w:
        player.value.direction = "right"
        if (player.value.animateInterval == null) {
          player.value.animate()
        }
        leftStick.style.transform = "translate(25px, -25px)"
        this.offsetX -= speed / 1.5
        this.offsetY += speed / 1.5
        player.value.x += speed / 1.5
        player.value.y -= speed / 1.5

        for (let i = 0; i < this.objects.length; i++) {
          const obj = this.objects[i]
          if (player.value.x / 12 <= obj.collider.x + obj.collider.width && (player.value.x + player.value.width * 2) / 12 >= obj.collider.x && player.value.y / 12 <= obj.collider.y + obj.collider.height && (player.value.y + player.value.height * 2) / 12 >= obj.collider.y) {
            this.offsetX += speed / 1.5
            this.offsetY -= speed / 1.5
            player.value.x -= speed / 1.5
            player.value.y += speed / 1.5
          }
        }

        break;
      case keys.d && keys.s:
        player.value.direction = "right"
        if (player.value.animateInterval == null) {
          player.value.animate()
        }
        leftStick.style.transform = "translate(25px, 25px)"
        this.offsetX -= speed / 1.5
        this.offsetY -= speed / 1.5
        player.value.x += speed / 1.5
        player.value.y += speed / 1.5

        for (let i = 0; i < this.objects.length; i++) {
          const obj = this.objects[i]
          if (player.value.x / 12 <= obj.collider.x + obj.collider.width && (player.value.x + player.value.width * 2) / 12 >= obj.collider.x && player.value.y / 12 <= obj.collider.y + obj.collider.height && (player.value.y + player.value.height * 2) / 12 >= obj.collider.y) {
            this.offsetX += speed / 1.5
            this.offsetY += speed / 1.5
            player.value.x -= speed / 1.5
            player.value.y -= speed / 1.5
          }
        }

        break;
      case keys.a:
        player.value.direction = "left"
        if (player.value.animateInterval == null) {
          player.value.animate()
        }
        leftStick.style.transform = "translate(-25px, 0)"
        this.offsetX += speed
        player.value.x -= speed

        for (let i = 0; i < this.objects.length; i++) {
          const obj = this.objects[i]
          if (player.value.x / 12 <= obj.collider.x + obj.collider.width && (player.value.x + player.value.width * 2) / 12 >= obj.collider.x && player.value.y / 12 <= obj.collider.y + obj.collider.height && (player.value.y + player.value.height * 2) / 12 >= obj.collider.y) {
            this.offsetX -= speed
            player.value.x += speed
          }
        }

        break;
      case keys.d:
        player.value.direction = "right"
        if (player.value.animateInterval == null) {
          player.value.animate()
        }
        leftStick.style.transform = "translate(25px, 0)"
        this.offsetX -= speed
        player.value.x += speed

        for (let i = 0; i < this.objects.length; i++) {
          const obj = this.objects[i]
          if (player.value.x / 12 <= obj.collider.x + obj.collider.width && (player.value.x + player.value.width * 2) / 12 >= obj.collider.x && player.value.y / 12 <= obj.collider.y + obj.collider.height && (player.value.y + player.value.height * 2) / 12 >= obj.collider.y) {
            this.offsetX += speed
            player.value.x -= speed
          }
        }

        break;
      case keys.w:
        player.value.direction = "up"
        if (player.value.animateInterval == null) {
          player.value.animate()
        }
        leftStick.style.transform = "translate(0, -25px)"
        this.offsetY += speed
        player.value.y -= speed

        for (let i = 0; i < this.objects.length; i++) {
          const obj = this.objects[i]
          if (player.value.x / 12 <= obj.collider.x + obj.collider.width && (player.value.x + player.value.width * 2) / 12 >= obj.collider.x && player.value.y / 12 <= obj.collider.y + obj.collider.height && (player.value.y + player.value.height * 2) / 12 >= obj.collider.y) {
            this.offsetY -= speed
            player.value.y += speed
          }
        }

        break;
      case keys.s:
        player.value.direction = "down"
        if (player.value.animateInterval == null) {
          player.value.animate()
        }
        leftStick.style.transform = "translate(0, 25px)"
        this.offsetY -= speed
        player.value.y += speed

        for (let i = 0; i < this.objects.length; i++) {
          const obj = this.objects[i]
          if (player.value.x / 12 <= obj.collider.x + obj.collider.width && (player.value.x + player.value.width * 2) / 12 >= obj.collider.x && player.value.y / 12 <= obj.collider.y + obj.collider.height && (player.value.y + player.value.height * 2) / 12 >= obj.collider.y) {
            this.offsetY += speed
            player.value.y -= speed
          }
        }

        break;
      default:
        clearInterval(player.value.animateInterval)
        walkingSound.pause()
        leftStick.style.transform = "translate(0, 0)"
        player.value.animateInterval = null
        player.value.currentAnimation = 0
        break;
    }
    // this.world.style.transform = `translate(${this.offsetX}px, ${this.offsetY}px) scale(1)`
    player.value.draw()
  }

  click(id) {
    this.objects.forEach(el => {
      if (el.id != id) return
      el.click()
    })
  }

  grow() {
    this.objects.forEach(el => {
      if (!el.plantingTime) return
      if (el.level == el.maxLevel) return
      console.log(el)
      let now = Date.now()
      if (now - el.plantingTime > plantsGrowthSpeed[el.type]) {
        el.level++
        el.plantingTime = new Date()
      }
    })
  }
}



onMounted(() => {

  player.value = new Player()
  world.value = new Map(player.value)
  leftStick = document.querySelector('#left_stick')
  setInterval(() => {
    world.value.animate()
  }, 1000 / 60)
  world.value.objects.forEach(el => {
    if (el instanceof Animated) el.animate()
  })
  setInterval(() => {
    world.value.grow()
  }, 1000)
  load()
  window.addEventListener('beforeunload', save)
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
  transition: all 0.3s;
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
  transition: all 0.3s;
  box-shadow: inset -1px 1px 0.6px rgba(255, 255, 255, 0.250), -1px 1px 7px 4px rgba(0, 0, 0, 0.250);
}

.button:active {
  box-shadow: inset -1px 1px 0.6px rgba(255, 255, 255, 0.250), -1px 1px 7px 0px rgba(0, 0, 0, 0.250);
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
  left: -90px;
  top: -12px;
}

.house {
  position: absolute;
  width: 48px;
  transition: all 0.3s;
}



.player {
  width: 13px;
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
  font-size: 24px;
  display: flex;
  justify-content: center;
  align-items: center;
  gap: 5px;
}

.money>img {
  width: 24px;
}

.money * {
  transform: none;
  position: relative;
}

.screen_edge {
  position: relative;
}

::-webkit-scrollbar {
  display: none;
}

.inventory {
  position: absolute;
  top: 0;
  left: 0;
  padding: 15px;
  background-color: #efca88;
  border: 5px solid #b89a67;
  z-index: 100;
  transform: none;
  border-radius: 15px;
  height: 100%;
  transition: all 0.3s;
}

.inventory * {
  transform: none;
  position: relative;
}

.inventory_hidden {
  transform: translateX(-100%);
}

.backpack_container {
  display: flex;
  flex-direction: column;
  gap: 15px;
  height: 100%;
  overflow-y: scroll;
}

.backpack_icon {
  padding: 5px;
  padding-left: 10px;
  display: flex;
  justify-content: center;
  align-items: center;
  position: absolute;
  top: 25px;
  right: -69px;
  background-color: #efca88;
  border: 5px solid #b89a67;
  border-left: none;
  border-radius: 0 15px 15px 0;
  cursor: pointer;
}

.backpack_icon>img {
  width: 50px;
}

.item {
  display: flex;
  height: 110px !important;
  width: 110px;
  flex-direction: column;
  align-items: center;
  justify-content: space-between;
  font-weight: bold;
  gap: 5px;
  padding: 15px;
  border-radius: 15px;
  box-shadow: inset 0 0 10px 0px rgba(0, 0, 0, 0.5);
  cursor: pointer;
}

.item img {
  width: 40px;
}

.item_image {
  position: relative;
}

.item_image>p {
  position: absolute !important;
  bottom: -5px;
  right: -5px;
  /* padding: 5px; */
  border: 2px solid #b89a67;
  background-color: #efca88;
  border-radius: 50px;
}

.item>p {
  width: min-content;
  text-align: center;
  line-height: 12px;
}

.picked_item {
  box-sizing: border-box;
  box-shadow: inset 0 0 10px 0px rgba(0, 0, 0, 0.5), 0 0 0 2px white;
}

.alert_container {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  display: flex;
  justify-content: center;
  align-items: center;
  z-index: 10;
  transform: none;
}

.alert_box * {
  transform: none;
}

.alert_box {
  z-index: 100;
  padding: 5px;
  border: 2px solid #b89a67;
  background-color: #efca88;
  border-radius: 5px;
  font-size: 0.7em;
  color: #b89a67;
  position: relative;
  display: flex;
  justify-content: center;
  align-items: center;
  width: fit-content;
  height: fit-content;
}

.alert_box>* {
  position: relative;
}

.alert_box::before {
  content: '';
  bottom: -8px;
  width: 10px;
  height: 10px;
  background-color: #efca88;
  border: 2px solid #b89a67;
  border-top: none;
  border-left: none;
  rotate: 45deg;
  position: absolute;
}

.v-enter-active,
.v-leave-active {
  transition: opacity 0.5s ease;
}

.v-enter-from,
.v-leave-to {
  opacity: 0;
}

div.shop {
  z-index: 100;
  transform: none;
  position: absolute;
  bottom: 0;
  left: 0;
  width: 100%;
  height: 200px;
  padding: 25px;
  background-color: #efca88;
  border: 5px solid #b89a67;
  display: flex;
  flex-direction: column;
  gap: 15px;
  border-radius: 15px;
  transition: all 0.3s;
}

.shop_hidden {
  transform: translateY(100%) !important;
}

.shop_inside {
  display: flex;
  justify-content: space-between;
  height: 100%;
}

div.shop * {
  transform: none;
  position: relative;
}

.trader_portrait {
  height: 150px;
  margin-top: -25px;
}

.shop_items {
  height: 120px;
  width: 100%;
  display: grid;
  grid-template-columns: 1fr 1fr 1fr 1fr;
  gap: 15px;
  overflow-y: scroll;
}

div.shop .item {
  width: 100%;
  height: 100%;
}

.shop_header {
  display: flex;
  gap: 15px;
}

div.shop button {
  padding: 5px 10px;
  border-radius: 10px;
  background-color: #efca88;
  border: none;
  transition: all 0.3s;
  box-shadow: 0 0 10px 0px rgba(0, 0, 0, 0.5);
  cursor: pointer;
}

div.shop button.active {
  box-shadow: 0 0 3px 0px rgba(0, 0, 0, 0.5);
}

.trader {
  transition: all 0.3s;
}

.trader:hover {
  transform: scale(2.3);
}




.props {
  display: flex;
  justify-content: center;
  align-items: center;
  cursor: pointer;
}

.props>h3 {
  display: none !important;
  line-height: 6px;
  padding: 3px;
  display: flex;
  justify-content: center;
  align-items: center;
  font-size: 6px;
  z-index: 90;
  background-image: url('./assets/coin.png');
  background-size: contain;
  background-repeat: no-repeat;
  background-position: center;
}

.props:hover h3 {
  display: block !important;
}

.props>img {
  transform: none;
  position: relative;
}
</style>
