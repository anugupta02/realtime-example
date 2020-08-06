<!--eslint-disable-->
<template>
  <div class="container">
    <div class="feed-container" id="feed">
      <div class="simple-h3">Feed</div>
      <div v-for="link of links" :key="link.url" class="simple-alert simple-alert--info animated fadeInDown">
        {{link.title }}: <span class="simple-link"> {{link.url}} </span>
      </div>
    </div>
  </div>
</template>

<script>
import {Socket} from 'phoenix-socket'

export default{
  name: 'Feed',
  mounted () {
    let socket = new Socket('ws://localhost:4000/socket')
    socket.connect()

    this.channel = socket.channel('links', {})

    this.channel.join()
    .receive('ok', resp => { console.log('Feed Joined successfully', resp) })
    .receive('error', resp => { console.log('Feed Unable to join', resp) })

    this.channel.on('new_link_added', payload => {
      console.log('from Phoenix', payload)
      this.links.push(payload.link)
    })
  },
  data () {
    return {
      links: []
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
    width: 80%;
  }

  .feed-container {
    width: 50%;
    height: auto;
    background: white;
    border-radius: 5px;
    display: flex;
    flex-direction: column;
    justify-content: space-around;
    align-items: center;
    padding: 50px;
    text-align: center;
    box-shadow: 1px 2px 5px #dbdbdb;

  }

  .simple-alert {
    letter-spacing: 1.5px;
     width: 50%;
  }
  .simple-a, .simple-link {
    color: #fff;
  }
</style>
