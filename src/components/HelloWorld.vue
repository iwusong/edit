<script setup>
import {nextTick, ref} from 'vue'

const vmtext = ref(null)
const dom = ref(null)

const selectionindex = ref(null)

const DATAmap = {}

function clickbutton(i, color) {

  DATAmap[i] = color
  let text = dom.value.innerText
  if (selectionindex) {
    let s1 = text.slice(0, selectionindex.value)
    let s2 = text.slice(selectionindex.value)
    dom.value.innerText = s1 + i + s2
    vmtext.value = dom.value.innerText
  }
}

function blur1() {
  selectionindex.value = getSelection()?.anchorOffset
}

function output(text) {
  for (const datAmapKey in DATAmap) {
    text = text.replaceAll(datAmapKey, `<span style="color: ${DATAmap[datAmapKey]}">${datAmapKey}</span>`)
  }
  console.log(text)
  return text
}

</script>

<template>
  光标{{ selectionindex }}
  <button @click="clickbutton('vue','red')" style="color: red">vue</button>
  <button @click="clickbutton('js','green')" style="color: green">js</button>
  <button @click="clickbutton('css','blue')" style="color:blue">css</button>

  <div>
    <div @blur="blur1" @input="e=>vmtext=e.target.innerText" @keydown.enter="e=>e.preventDefault()" ref="dom"
         contenteditable="true"
         style="width: 300px;height:100px;border: 1px black solid;position: absolute;color: rgba(0,0,0,0)"></div>
    <div v-html="output(vmtext)"
         style="width: 300px;height:100px;border: 1px black solid;position: absolute;pointer-events: none ">
    </div>
  </div>

</template>

<style scoped>
button {
  margin: 20px;
}

div {
  caret-color: black;
}
</style>
