<script setup>
import {nextTick, ref} from 'vue'

const vmtext = ref(null)
const dom = ref(null)

const selectionindex = ref(null)

const DATAmap = {}

function clickbutton(i, color) {
  i = "$" + i + "$"
  DATAmap[i] = color
  let text = dom.value.innerText
  if (selectionindex) {
    let s1 = text.slice(0, selectionindex.value)
    let s2 = text.slice(selectionindex.value)
    dom.value.innerText = s1 + i + s2
    vmtext.value = dom.value.innerText
  }
}

function calculaIndex(e) {
  selectionindex.value = getSelection()?.anchorOffset
}

function doInput(e) {
  vmtext.value = e.target.innerText
  calculaIndex()
}

function output(text) {
  for (const datAmapKey in DATAmap) {
    text = text.replaceAll(datAmapKey, `<span style="color: ${DATAmap[datAmapKey]}">${datAmapKey}</span>`)
  }

  return text
}

function deleteOne(e) {
  let allString = vmtext.value
  const lastString = allString.slice(selectionindex.value - 1, selectionindex.value)
  console.log(lastString)
  if (lastString === "$") {
    e.preventDefault()
    let list = Array.from(allString.slice(0, selectionindex.value - 1))
    console.log(list)
    let i = 1
    while (list.length) {
      i++
      if (list.pop() === "$") {
        const start = selectionindex.value - i
        const end = selectionindex.value
        vmtext.value = allString.slice(0, start) + allString.slice(end)
        dom.value.innerText=vmtext.value
      }
    }

  }

}
</script>

<template>
  光标{{ selectionindex }}
  <button @click="clickbutton('创建人姓名','red')" style="color: red">创建人姓名</button>
  <button @click="clickbutton('创建人id','green')" style="color: green">创建人id</button>


  <div>
    <div @keydown.delete="deleteOne" @click="calculaIndex" @keydown="calculaIndex" @input="doInput"
         @keydown.enter="e=>e.preventDefault()" ref="dom"
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
