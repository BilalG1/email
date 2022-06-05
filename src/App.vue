<template>
<div class="container">
  <div v-for="inp in Object.keys(formData)" :key="inp" class="row">
    <span class="label">
      {{inp}}
    </span>
    <input type="text" class="input" v-model="formData[inp]" />
  </div>
</div>
  <button class="submit" @click="post">
    Send
  </button>
  <div v-if="response" class="response">
    Response: {{ response }}
  </div>
</template>

<script>

export default {
  name: 'App',
  data() {
    return {
      formData: {
        fromEmail: '',
        fromName: '',
        toEmail: '',
        toName: '',
        subject: '',
        message: '',
      },
      response: '',
    }
  },
  methods: {
    post() {
      const url = process.env.NODE_ENV == 'production' ? 'https://mail.bg-cloud.workers.dev/' : 'http://localhost:63440/'
      const service = url + 'api/send'
      const options = {
        method: 'post',
        body: JSON.stringify(this.formData),
        headers: {'Content-Type': 'application/json'}
      }

      fetch(service, options)
      .then(res => res.text())
      .then(data => {
        console.log(data)
        this.response = data
      })
      .catch(e => {
        this.response = e
      })
    }
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
.label {
  font-size: 18px;
    font-weight: 600;
    margin-right: 20px;
}
.submit {
      border-radius: 8px;
    border: 1px solid black;
    padding: 10px 20px;
    background: white;
    color: black;
    margin: 20px 0;
    cursor: pointer;
    transition: .2s all;
}
.input {
  font-size: 16px;
      padding: 3px 6px;
    color: gray;
}
.row {
  margin-bottom: 10px;
  display: flex;
    justify-content: space-between;
}
.submit:hover {
  background: lightgray;
}
.container {
  width: fit-content;
  margin: 0 auto;
  padding: 20px;
    border-radius: 10px;
    box-shadow: rgb(100 100 111 / 20%) 0px 7px 29px 0px;

}
.response {
  font-family: monospace;
    width: 70vw;
    margin: 0 auto;
    text-align: left;
    max-width: 600px;
}
</style>
