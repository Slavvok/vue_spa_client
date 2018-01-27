<template>
  <div class="home">
    <h1>Home</h1>
    <div class="messages">
      <div id="message" v-for="message in messages">
        <h3>{{message.date}}</h3>
        <h2>{{message.text}}</h2>
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
      counter: 0
    }
  },
  created: function(){
    this.getMessages();
    setInterval(getMessages, 2000);
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
    changeState: function() {
    var self = this;
    axios.put('https://slavvok.pythonanywhere.com/api/get_messages/{messages.id}')
    .then(response => {
      self.messages.isread = true
    })
    }
  },
}

</script>

<style>
#message{
  border: 2px solid;
  margin-top: 10px;
  text-align: center;
}
</style>
