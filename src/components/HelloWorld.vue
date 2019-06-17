<template>
    <div class="container text-center pt-5 mt-5 ">
        <div v-if="status" class="container" 
            style="    
              position: fixed;
              width: 100%;
              height: 100%;
              top: 0;
              left: 0;
              right: 0;
              display: flex;
              justify-content: center;
              align-items: center;
              z-index: 2;">
          <div class="spinner-border text-primary" role="status">
            <span class="sr-only">Loading...</span>
          </div>
        </div>
        <div v-if="status" class="container" 
            style="    
              background: #e2e2e2b8;
              position: fixed;
              width: 100%;
              height: 100%;
              top: 0;
              left: 0;
              right: 0;
              display: flex;
              justify-content: center;
              align-items: center;
              z-index: 1;
              filter: blur(20px);"></div>
        <form class="mt-5 w-75 d-inline-block text-left border order-primary p-5 rounded shadow bg-light ">
          <div>
            <div class="form-group">
                <label for="exampleInputEmail1">Correo Electrónico</label>
                <input v-model="emailToSend" @change="statusMessage = ''" type="email" class="form-control" id="l_Username" placeholder="Ingresa correo electrónico">
            </div>
             <div class="form-group">
                <label for="exampleInputEmail1">Titulo</label>
                <input v-model="titleSend" type="text" class="form-control" placeholder="Ingresa el asunto">
            </div>
            <div class="form-group">
              <label for="exampleFormControlTextarea1">Mensaje</label>
              <textarea v-model="messageToSend" class="form-control" id="exampleFormControlTextarea1" rows="3" placeholder="tu mensaje aqui"></textarea>
            </div>
            <div class="text-center">
                <a @click="eSend" class="btn btn-primary w-50 p-3 shadow-sm text-light">Enviar</a>
            </div>
          </div>
        </form>
        <div v-if="statusMessage" class="text-primary m-4" style="background: #ffffffc7;
        padding: 2%;
        border-radius: 25px;
        text-transform: uppercase;
        font-weight: 600;">{{statusMessage}}</div>
    </div>
</template>

<script>
const axios = require('axios')
const URL = 'https://ijdplfetc0.execute-api.us-east-1.amazonaws.com/default/email-send-grid'

export default {
  name: 'HelloWorld',
  props: {
    msg: String
  },
  data () {
    return {
      emailToSend: '',
      messageToSend: '',
      titleSend: '',
      results: [],
      statusMessage: '',
      status: false
    }
  },
  methods: {
    eSend: function() {
      if(this.emailToSend && this.messageToSend ) {
        let emailSend = JSON.stringify({
                          to: this.emailToSend,
                          subject: this.titleSend,
                          message: this.messageToSend
                        })
        var config = {
          headers: { 'Content-Type': 'application/json' }
        };
        this.status = true;
        axios.post(URL, emailSend, config)
              .then((res) => {
                this.statusMessage = res.data
                this.clearInput();
                this.status = false;
              })
              .catch(function (error) {
                console.log(error)
                this.status = false;
              })
      }
    },
    clearInput: function () {
      this.emailToSend = '',
      this.messageToSend = '',
      this.titleSend = ''
    }

  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}

div.container {
   filter: blur(-10px) !important;
}
</style>
