<template>
  <h1>Omniwheel Controller</h1>
  <ButtonJoystick @up="forward" @down="backward" @left="left" @right="right"
  @rel_up="stop" @rel_down="stop" @rel_left="stop" @rel_right="stop"/>
  <ButtonJoystick @up="forward" @down="backward" @left="turnLeft" @right="turnRight"
  @rel_up="stop" @rel_down="stop" @rel_left="stop" @rel_right="stop"/>
</template>

<script>

import ButtonJoystick from './components/ButtonJoystick.vue'

export default {
  name: 'App',
  components: {
    ButtonJoystick
  },
  data: function() {
    return {
      connection: null,
    }
  },
  methods: {
    forward() {
      console.log("button forward pressed")
      this.connection.send("V")
    },
    backward() {
      console.log("button backward pressed")
      this.connection.send("B")
    },
    left() {
      console.log("button left pressed")
      this.connection.send("L")
    },
    right() {
      console.log("button right pressed")
      this.connection.send("R")
    },
    turnLeft() {
      console.log("button turnLeft pressed")
      this.connection.send("l")
    },
    turnRight() {
      console.log("button turnRight pressed")
      this.connection.send("r")
    },
    stop() {
      console.log("button released")
      this.connection.send("S")
    }
  },
  created: function() {
    console.log("Starting connection to WebSocket Server");

    try {
      this.connection = new WebSocket('ws://192.168.178.45:8765');
    } catch {
      console.log("Could not establish connection.")
    }

    this.connection.onmessage = function(event) {
      console.log(event);
    }

    this.connection.onopen = function(event) {
      console.log(event)
      console.log("Successfully connected to the echo websocket server...")
    }

    this.connection.onclose = function(event) {
      console.log(event);
    }

    console.log(this.connection);
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

table {
  margin-left: auto;
  margin-right: auto;
}

button {
  background-color: #04AA6D;
  border: none;
  color: white;
  
  text-align: center;
  text-decoration: none;
  display: inline-block;
  font-size: 16px;
  margin: 4px 2px;
  border-radius: 50%;
  height:50px;
  width:50px;
}
</style>
