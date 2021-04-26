<template>
  <div class="container">
    <div>
      <vue-qrcode v-if="!connected && qrcode" :value="qrcode" :options="{ width: 200 }"></vue-qrcode>
      <h1 class="title">
        WhatsApp Bot 
        <div v-if="!connected">Aguardando conexão </div>
        <div v-if="connected">Conectado, peça para alguém lhe mandar o texto: <code>!ping</code> </div>
      </h1>
    </div>
  </div>
</template>

<script>
import { io } from "socket.io-client";

export default {
  data: () => ({
    connected: false,
    qrcode: null,
  }),
  mounted() {
    const socket = io(process.env.URL_WEBSOCKET);

    socket.on('configs', (msg) => {
      this.connected = msg.status;
      this.qrcode = msg.qrcode;
    })
  }
}
</script>

<style>
.container {
  margin: 0 auto;
  min-height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  text-align: center;
}

.title {
  font-family:
    'Quicksand',
    'Source Sans Pro',
    -apple-system,
    BlinkMacSystemFont,
    'Segoe UI',
    Roboto,
    'Helvetica Neue',
    Arial,
    sans-serif;
  display: block;
  font-weight: 300;
  font-size: 20px;
  color: #35495e;
  letter-spacing: 1px;
}

.subtitle {
  font-weight: 300;
  font-size: 42px;
  color: #526488;
  word-spacing: 5px;
  padding-bottom: 15px;
}

.links {
  padding-top: 15px;
}
</style>
