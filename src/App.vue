<template>
  <div id="app">
    <div>
      <input v-model="room">
      <button @click="join">join</button>
      <br>
      <input v-model="message">
    <button @click="createNewMessage">Send</button>
      <ul v-for="(value,index) in listMessage" :key="index">
        <li>{{value}}</li>
      </ul>
    </div>
  </div>
</template>

<script>

export default {
  name: 'app',
  data(){
    return{
      listMessage: [],
      message: '',
      room: ''
    }
  },
  created(){
    this.sockets.subscribe('room-message',(data)=>{
      console.log(data)
      this.listMessage = [...this.listMessage,data];
    })
  },
  sockets: {
    connect() {
      console.log('connectwwwww')
    },

    disconnect() {
      console.log('disconnectssssss')
    }
  },
  methods:{
    createNewMessage(){
      console.log('sss')
      this.$socket.emit('message',{message:this.message,room:this.room});
      this.listMessage.push(`You : ${this.message}`)
    },
    join(){
      this.$socket.emit('join',this.room);
    }
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
