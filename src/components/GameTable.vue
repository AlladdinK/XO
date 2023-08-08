<template>
  <div class="game-table container" v-if="list">
    <button @click="chooser(item, idx)" v-for="item, idx in list " :key="item" class="game-table__item"></button>

  </div>
</template>
<script setup>
import imgO from '../assets/img/o.svg'
import imgX from '../assets/img/x.svg'
import { ref } from 'vue'
let list = ref([])
let result = ref([[1, 2, 3], [4, 5, 6], [7, 8, 9], [1, 4, 7], [2, 5, 8], [3, 6, 9], [1, 5, 9], [3, 5, 7]])
for (let i = 0; i < 9; i++) {
  list.value.push({
    check: false,
    checker: 'red',
    person: ''
  })
}
let person1 = 'Alex'
let person2 = 'Jason'
let current = person1
let count = 0
function chooser(item, idx) {

  let itemElem = document.querySelectorAll('.game-table__item')
  if (!item.check) {
    if (current == person1 && item.person != person2) {
      itemElem[idx].style.background = `url(${imgX})`
      itemElem[idx].style.backgroundRepeat = `no-repeat`
      itemElem[idx].style.backgroundSize = `50%`
      itemElem[idx].style.backgroundPosition = `center`
      item.check = ref(true).value
      item.person = person1
      current = person2
    } else if (current == person2 && item.person != person1) {
      itemElem[idx].style.background = `url(${imgO})`
      itemElem[idx].style.backgroundRepeat = `no-repeat`
      itemElem[idx].style.backgroundSize = `50%`
      itemElem[idx].style.backgroundPosition = `center`
      item.check = ref(true).value
      item.person = person2
      current = person1
    }
    if (item.check) {
      count++
    }

    personWin()
  }
}
function personWin() {
  for (let i = 0; i < result.value.length; i++) {
    let pers1 = 0
    let pers2 = 0

    result.value[i].forEach((el) => {
      if (list.value[el - 1].check && list.value[el - 1].person == person1) {
        pers1++
      } else if (list.value[el - 1].check && list.value[el - 1].person == person2) {
        pers2++
      }
    })
    if (pers1 == 3) {
      pers1 = 0
      setTimeout(() => {
        endGame(person1)
      }, 0);
      break
    } else if (pers2 == 3) {
      pers2 = 0
      setTimeout(() => {
        endGame(person2)
      }, 0);
      break
    } else if (count == 9 && pers1 < 3 && pers2 < 3) {
      count = 0
      setTimeout(() => {
        endGame()
      }, 0);
      break
    }


  }

}
function endGame(winner) {
  list.value.forEach((el) => {
    el.check = false
    el.person = ''
  })
  let itemElem = document.querySelectorAll('.game-table__item')
  itemElem.forEach((el) => {
    el.style.background = 'transparent'
  })
  count = 0
  if (winner) {
    alert(winner)
  } else {
    alert('Ничья')
  }
}



</script>
<style lang="scss">
.game-table {
  display: grid;
  grid-template-columns: repeat(3, 100px);
  grid-template-rows: repeat(3, 100px);
  justify-content: center;
  margin-top: 100px;

  &__item {
    border: 1px solid #ccc;
    background: rgba(255, 255, 255, 0.4);
    cursor: pointer;


  }
}
</style>