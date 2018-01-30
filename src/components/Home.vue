<template>
  <div class="home">
    <h1>Home</h1>
    <div class="messages">
      <div id="message" v-for="message in messages">
        <h3>{{message.date}}</h3>
        <h3>{{message.pk}}</h3>
        <h2>{{message.text}}</h2>
        <button v-on:click="changeState(message)"> Read </button>
        <h2>{{message.isread}}</h2>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  name: 'Home',
  data: function(){
    return {
      messages: [],
      interval: null,
    }
  },
  created: function(){
    this.getMessages();
    setInterval( function() {
      this.getMessages();
    }.bind(this), 10000);
  },
  methods: {
    getMessages: function() {
      var self = this;
      axios.get('https://slavvok.pythonanywhere.com/api/get_messages/')
      .then(response => {
        self.messages = response.data;
        })
      .catch(error => {
      console.log(error);
      });
  },
    changeState: function(message) {
      var self = this;
      axios.get(`https://slavvok.pythonanywhere.com/api/get_messages/${message.pk}/mark_read/`)
      .then(response => {
        self.message = response.data;
      })
      self.getMessages();
    },
  },
    beforeDestroy: function(){
      clearInterval(this.interval);
  },
}

</script>

<style>
#message{
  border: 2px solid;
  margin: auto;
  margin-bottom:10px;
  width: 400px;
  text-align: center;
  background-color: #99ccff;
}
</style>
