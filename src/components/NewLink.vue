<!--eslint-disable-->
<template>
  <div class="container">
    <div class="form">
      <div class="simple-h3">Add a New Link </div>
      <div>
        <label class="simple-label">Title </label>
        <input  v-model="form.title" type="text" class="simple-input" placeholder="Github">
      </div>
      <div>
        <label class="simple-label">URL</label>
        <input v-model="form.url" type="text" class="simple-input" placeholder="https://github.com/">
      </div>
      <div class="save-button-container">
        <div @click="addLink" class="simple-btn simple-btn--blue">Add</div>
      </div>
    </div>
  </div>
</template>

<script>
import {Socket} from 'phoenix-socket'

export default{
  name: 'NewLink',
  mounted () {
    let socket = new Socket('ws://localhost:4000/socket')
    socket.connect()

    this.channel = socket.channel('links', {})
    this.channel.join()
    .receive('ok', resp => { console.log('NewLink Joined successfully', resp) })
    .receive('error', resp => { console.log('NewLink Unable to join', resp) })
  },
  data () {
    return {
      form: {title: '',
        url: ''
      }
    }
  },
  methods: {
    addLink: function () {
      let link = {
        title: this.form.title,
        url: this.form.url
      }
      this.channel.push('new_link', {link: link})
      console.log(this.channel.push('new_link', {link: link}))
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  .container {
    display: flex;
    flex-direction: column;
    align-items: center;
  }

  .form {
    width: 300px;
    height: auto;
    background: white;
    border-radius: 5px;
    display: flex;
    flex-direction: column;
    justify-content: space-around;
    padding: 50px;
    text-align: center;
    box-shadow: 1px 2px 5px #dbdbdb;
    position: sticky;
    top: 0;
  }

  .simple-input {
    height: 35px;
    margin: 15px;
  }

  .simple-btn--blue {
    background-color: #4894f0;
    color: #fff;
    width: 216px;
    height: 45px;
    margin: 15px;
    display: flex;
    justify-content: center;
    align-self: center;
    align-items: center;
    justify-content: space-around;
  }
  .save-button-container {
    display: flex;
    justify-content: center;
  }

</style>
