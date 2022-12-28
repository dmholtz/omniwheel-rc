<template>
  <v-container class="fill-height">

    <v-responsive class="d-flex align-center text-center fill-height">

      <h1 class="joystick">Heading</h1>

      <svg class="joystick" viewBox="0 0 8 8" width="100%">
        <circle class="joystick" cx="4" cy="4" r="3.5" fill="#00247D"/>
        <circle class="joystick"
                @mousedown="handleMousedown" 
                @mouseup="handleMouseup"
                @mousemove="handleMousemove"
                @mouseleave="handleMouseup"
                @touchstart="touchStart"
                @touchend="touchEnd"
                @touchmove="touchMove" 
                :cx="4+joystickXDisplacement" :cy="4-joystickYDisplacement" r="2" fill="#CE1126"/>
      </svg>
      <div>{{ Math.round(joystickXDisplacement*100)/100 }} : {{ Math.round(joystickYDisplacement*100)/100 }}</div>

    </v-responsive>
  </v-container>
</template>

<script setup>
import { defineEmits, ref } from 'vue';

const emit = defineEmits(['joystick-move'])

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
  console.log("Touch Joystick")
  isClicked.value = true
  mouseXPos.value = mouseEvent.offsetX
  mouseYPos.value = mouseEvent.offsetY
}

function handleMouseup() {
  console.log("Release Joystick")
  isClicked.value = false
  updateJoystickDisplacement(0,0)
}

function handleMousemove(mouseEvent) {
  if (isClicked.value) {
    const deltaX = mouseEvent.offsetX - mouseXPos.value;
    const deltaY = mouseEvent.offsetY - mouseYPos.value;
    updateJoystickDisplacement(deltaX/15, deltaY/15)
  }
}


function touchStart(touchEvent) {
  touchXPos.value = touchEvent.touches[0].screenX
  touchYPos.value = touchEvent.touches[0].screenY

  touchState.value = "touchstart"
}

function touchMove(touchEvent) {
  const deltaX = touchEvent.touches[0].screenX - touchXPos.value
  const deltaY = touchEvent.touches[0].screenY - touchYPos.value

  updateJoystickDisplacement(deltaX/50, -deltaY/50);
  //joystickXPos.value = saturation(deltaX/50 + 5, 3, 7)
  //joystickYPos.value = saturation(deltaY/50 + 5, 3, 7)

  //touchState.value = "move"
}

function touchEnd(touchEvent) {
  //touchState.value = "end"
  updateJoystickDisplacement(0, 0)
}

function saturation(val, minVal, maxVal) {
  if (val < minVal) {
    return minVal;
  } else if (val > maxVal) {
    return maxVal;
  }
  else {
    return val;
  }
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
  background: green;
}
</style>
