<template>
  <v-container class="fill-height">

    <v-responsive class="d-flex align-center text-center fill-height">

      <div class="text-body-2 font-weight-light mb-n1">Welcome to</div>

      <h1 class="text-h2 font-weight-bold">Omniwheel Controller</h1>

      <div class="py-7"/>
      
      <v-row class="d-flex align-center justify-center">
        <v-col cols="auto">
          <DiscreteJoystick/>
        </v-col>
        <v-col cols="auto">
          <DiscreteJoystick/>
        </v-col>
        <v-col cols="auto">
          <TouchJoystick @joystick-move="handleJoystick"/>
        </v-col>
      </v-row>
    </v-responsive>
  </v-container>
</template>

<script setup>
import { onMounted, ref } from 'vue';
import DiscreteJoystick from './DiscreteJoystick.vue';
import TouchJoystick from './TouchJoystick.vue';

const connection = ref()

onMounted(() => {
  connection.value = new WebSocket('ws://192.168.178.45:8765');
  
  connection.value.onmessage = function(event) {
    console.log(event);
  }

  connection.value.onopen = function(event) {
    console.log(event)
    console.log("Successfully connected to the echo websocket server...")
  }

  connection.value.onclose = function(event) {
    console.log(event);
  }
})

function handleJoystick(joystickEvent) {
  if (joystickEvent.x == 0 && joystickEvent.y == 0) {
    connection.value.send("S")
  } else {
    connection.value.send(JSON.stringify(joystickEvent))
  }
}

function forward() {
  console.log("Hi")
  connection.value.send("V")
}

function stop() {
  console.log("stop")
  connection.value.send("S")
}

</script>
