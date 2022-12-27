<template>
  <v-container class="fill-height">

    <v-responsive class="d-flex align-center text-center fill-height">

      <v-row class="d-flex align-center justify-center">
        <v-col cols="auto">
          <v-btn @mousedown="forward" @mouseup="stop" v-touch="{start: () => forward(), end: stop}" size="x-large" fab>
            <v-icon icon="mdi-arrow-up" size="x-large"/>
          </v-btn>
        </v-col> 
      </v-row>

      <v-row class="d-flex align-center justify-center">
        <v-col cols="auto">
          <v-btn @mousedown="forward" @mouseup="stop" v-touch="{start: () => forward(), end: stop}" size="x-large" fab>
            <v-icon icon="mdi-arrow-left" size="x-large"/>
          </v-btn>
        </v-col>

        <v-col cols="auto"/>

        <v-col cols="auto">
          <v-btn @mousedown="forward" @mouseup="stop" v-touch="{start: () => forward(), end: stop}" size="x-large" fab>
            <v-icon icon="mdi-arrow-right" size="x-large"/>
          </v-btn>
        </v-col>
      </v-row>

      <v-row class="d-flex align-center justify-center">
        <v-col cols="auto">
          <v-btn @mousedown="forward" @mouseup="stop" v-touch="{start: () => forward(), end: stop}" size="x-large" fab>
            <v-icon icon="mdi-arrow-down" size="x-large"/>
          </v-btn>
        </v-col>
      </v-row>      
    </v-responsive>
  </v-container>
</template>

<script setup>
import { onMounted, ref } from 'vue';

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

function forward() {
  console.log("Hi")
  connection.value.send("V")
}

function stop() {
  console.log("stop")
  connection.value.send("S")
}

</script>
