<template>
  <div>
    <b-row class="justify-content-md-center mt-2">
      <h1>Demo P2P Chat</h1>
    </b-row>
    <b-row class="mt-2">
      <b-button block variant="outline-primary" to="/demo/host">Host</b-button>
    </b-row>
    <b-row class="mt-4">
      <b-button block variant="outline-primary" to="/demo/client">Client</b-button>
    </b-row>
    <b-row class="mt-4">
      This is just a P2P chat demo based off the PeerJS <a href="https://github.com/jmcker/Peer-to-Peer-Cue-System">Example</a>
      This is NOT encrypted, just that it's P2P and uses the peerjs default servers for session metadata and candidate signaling. First
      create a 'host', copy it's ID and then create a 'client', paste the host ID as receiverID and then you should be connected. Try
      sending messages to each other!
    </b-row>
  </div>
</template>

<script>
export default {
  asyncData() {
    return {}
  },
}
</script>
