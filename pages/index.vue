<template>
  <div>
    <b-row class="justify-content-md-center mt-2">
      <h1></h1>
    </b-row>
    <b-jumbotron header="Dub 2 Dub!" lead="Hello D2D Players! 👋">
      <p>The game is a WIP! I am still figuring out the game design and other stuff.</p>
      <p>
        The plan is to use <a href="https://nuxtjs.org/">NuxtJs</a>, <a href="https://peerjs.com/">PeerJS</a> and
        <a href="https://pages.github.com/">Github Pages</a> to build a P2P multiplayer party game, hopefully 🤞.
      </p>
      <p>In the meantime enjoy a P2P Chat app Demo!</p>
      <b-button variant="primary" href="/demo">P2P Chat Demo</b-button>
    </b-jumbotron>
  </div>
</template>

<script>
export default {
  asyncData() {
    return {}
  },
}
</script>
