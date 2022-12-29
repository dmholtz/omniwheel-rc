<template>
  <div>{{description}}</div>
  <svg class="joystick" viewBox="0 0 8 8" width="60%">
    <circle class="joystick" cx="4" cy="4" r="3.5" fill="#00247D"/>
    <circle class="joystick"
            @mousedown="handleMousedown" 
            @mouseup="handleMouseup"
            @mousemove="handleMousemove"
            @mouseleave="handleMouseup"
            @touchstart="handleTouchStart"
            @touchend="handleTouchEnd"
            @touchmove="handleTouchMove" 
            :cx="4+joystickXDisplacement" :cy="4-joystickYDisplacement" r="2" fill="#CE1126">  
    </circle>
    <svg width="2" height="2" :x="3+joystickXDisplacement" :y="3-joystickYDisplacement" fill="currentColor" viewBox="0 0 16 16">
      <path fill-rule="evenodd" d="M7.646.146a.5.5 0 0 1 .708 0l2 2a.5.5 0 0 1-.708.708L8.5 1.707V5.5a.5.5 0 0 1-1 0V1.707L6.354 2.854a.5.5 0 1 1-.708-.708l2-2zM8 10a.5.5 0 0 1 .5.5v3.793l1.146-1.147a.5.5 0 0 1 .708.708l-2 2a.5.5 0 0 1-.708 0l-2-2a.5.5 0 0 1 .708-.708L7.5 14.293V10.5A.5.5 0 0 1 8 10zM.146 8.354a.5.5 0 0 1 0-.708l2-2a.5.5 0 1 1 .708.708L1.707 7.5H5.5a.5.5 0 0 1 0 1H1.707l1.147 1.146a.5.5 0 0 1-.708.708l-2-2zM10 8a.5.5 0 0 1 .5-.5h3.793l-1.147-1.146a.5.5 0 0 1 .708-.708l2 2a.5.5 0 0 1 0 .708l-2 2a.5.5 0 0 1-.708-.708L14.293 8.5H10.5A.5.5 0 0 1 10 8z"/>
    </svg>
  </svg>
  <div>{{ Math.round(joystickXDisplacement*100)/100 }} : {{ Math.round(joystickYDisplacement*100)/100 }}</div>
</template>

<script setup>
import { ref } from 'vue';

const emit = defineEmits(['joystick-move'])
const props = defineProps(['description'])

// mouse state
const isClicked = ref(false)
const mouseXPos = ref(NaN)
const mouseYPos = ref(NaN)

// touch state
const touchXPos = ref(NaN)
const touchYPos = ref(NaN)
const touchState = ref("Nothing")

// joystick displacement
const joystickXDisplacement = ref(0)
const joystickYDisplacement = ref(0)

// UI constants
const maxDisplacementRadius = 1

function handleMousedown(mouseEvent) {
  isClicked.value = true
  mouseXPos.value = mouseEvent.offsetX
  mouseYPos.value = mouseEvent.offsetY
}

function handleMouseup() {
  isClicked.value = false
  updateJoystickDisplacement(0,0)
}

function handleMousemove(mouseEvent) {
  if (isClicked.value) {
    const deltaX = mouseEvent.offsetX - mouseXPos.value;
    const deltaY = mouseEvent.offsetY - mouseYPos.value;
    // TODO optimize joystick displacement scaling
    updateJoystickDisplacement(deltaX/15, -deltaY/15)
  }
}


function handleTouchStart(touchEvent) {
  touchXPos.value = touchEvent.touches[0].screenX
  touchYPos.value = touchEvent.touches[0].screenY
}

function handleTouchMove(touchEvent) {
  const deltaX = touchEvent.touches[0].screenX - touchXPos.value
  const deltaY = touchEvent.touches[0].screenY - touchYPos.value
  // TODO optimize joystick displacement scaling
  updateJoystickDisplacement(deltaX/25, -deltaY/25);
}

function handleTouchEnd(touchEvent) {
  updateJoystickDisplacement(0, 0)
}

function updateJoystickDisplacement(xDisplacement, yDisplacement) {
  const displacementRadius = Math.sqrt(xDisplacement**2 + yDisplacement**2)
  touchState.value = displacementRadius + xDisplacement
  if (displacementRadius > maxDisplacementRadius) {
    const rescale = maxDisplacementRadius / displacementRadius
    joystickXDisplacement.value = xDisplacement * rescale
    joystickYDisplacement.value = yDisplacement * rescale
  } else {
    joystickXDisplacement.value = xDisplacement
    joystickYDisplacement.value = yDisplacement
  }
  console.log(joystickXDisplacement.value, joystickYDisplacement.value)
  emit('joystick-move', {x: joystickXDisplacement.value, y: joystickYDisplacement.value})
}

</script>

<style scoped>
.joystick {
  touch-action: none;
}
</style>
