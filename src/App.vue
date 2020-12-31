<template>
  <div id="app">
    <Container>
      <ChatWindow>
        <ChatMessage v-for="(mesasage, i) in messages"
        :key="i">
        </ChatMessage>
      </ChatWindow>
    </Container>
  </div>
</template>


<script>
import ChatMessage from './components/ChatMessage.vue'
import Container from './components/Container.vue'
import ChatWindow from './components/ChatWindow.vue'
export default {
  name: 'App',
  components: {
    Container,
    ChatMessage,
    ChatWindow
  },
  data(){
    return{
      messages:[
        {username:'', datetime:'', text:''}
      ]
    }
  },
  methods:{
    getUserData(){
      this.axios.get("http://37.77.104.246/api/chat/getmessages.php")
      .then( (response)=>{
          this.username=response.data.author;
          this.text=response.data.text;
          this.datetime=response.data.datetime;
        }
    )
  },
  sendmessage(){
    this.axios.post('http://37.77.104.246/api/chat/sendmessage.php', {
      author: this.username,
      text: this.text,
      datetime: this.datetime,
    })
    .then((response) => {
        this.messages = response.data;
    })
  }
},
mounted(){
  this.getUserData();
  this.sendmessage();
}
};
</script>

<style>

body {
  margin: 0;
  background-color: #f9f9fa;
}

</style>
