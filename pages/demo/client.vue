<template>
  <div>
    <b-row class="justify-content-center mt-2">
      <h1>Client</h1>
    </b-row>

    <b-row class="justify-content-center mt-2">
      <h2 v-if="peer === null">Not yet connected! 😢</h2>
      <h2 v-else>{{ peer.id }}</h2>
      <b-button pill @click="create" variant="outline-dark" class="ml-2">
        <b-icon icon="arrow-clockwise"></b-icon>
      </b-button>
    </b-row>

    <b-row class="justify-content-center mt-2">
      <b-form inline>
        <b-form-input v-model="receiverId" placeholder="Receiver ID" class="mr-2"></b-form-input>
        <b-button variant="primary" @click="connect">Connect</b-button>
      </b-form>
    </b-row>

    <template v-if="conn !== null">
      <b-row class="justify-content-center mt-2">
        <h3>Connected to {{ conn.peer }}</h3>
      </b-row>
      <b-row class="justify-content-center mt-2">
        <b-form inline>
          <b-form-input v-model="sendMsg" placeholder="Send Message" class="mr-2"></b-form-input>
          <b-button variant="primary" @click="send">
            <b-icon icon="forward-fill"></b-icon>
          </b-button>
        </b-form>
      </b-row>
    </template>

    <b-row class="justify-content-center mt-4">
      <b-table sticky-header striped outlined hover :items="messages"></b-table>
    </b-row>
  </div>
</template>

<script>
function addMessage(ctx, msg, isSent) {
  let ts = new Date().toLocaleString()
  ctx.messages.unshift({
    time: ts,
    message: (isSent ? 'SELF: ' : 'PEER: ') + msg,
    _rowVariant: isSent ? 'danger' : 'primary',
  })
}

function joinPeer(ctx) {
  // Close old connection
  if (ctx.conn) {
    ctx.conn.close()
  }

  // ensure we have connection
  if (!ctx.peer) {
    createPeer(ctx)
  }

  // Create connection to destination peer specified in the input field
  ctx.conn = ctx.peer.connect(ctx.receiverId, {
    reliable: true,
  })

  ctx.conn.on('open', function () {
    console.log('Connected to: ' + ctx.conn.peer)
  })
  // Handle incoming data (messages only since this is the signal sender)
  ctx.conn.on('data', function (data) {
    addMessage(ctx, data, false)
  })

  ctx.conn.on('close', function () {
    console.log('Peer Connection destroyed')
  })
}

function sendPeer(ctx) {
  if (ctx.conn && ctx.conn.open) {
    ctx.conn.send(ctx.sendMsg)
    console.log(ctx.sendMsg + ' msg sent')
    addMessage(ctx, ctx.sendMsg, true)
    ctx.sendMsg = ''
  } else {
    console.log('Peer Connection is closed')
    reset(ctx)
  }
}

function reset(ctx) {
  if (ctx.peer) {
    ctx.peer.destroy()
  }
  ctx.peer = null
  ctx.lastPeerId = null
  ctx.conn = null
  ctx.messages = []
  ctx.receiverId = ''
  ctx.sendMsg = ''
}

function createPeer(ctx) {
  reset(ctx)
  let peer = new Peer(null, {
    debug: 2,
  })
  peer.on('open', function (id) {
    // Workaround for peer.reconnect deleting previous id
    if (peer.id === null) {
      console.log('Received null id from peer open')
      peer.id = ctx.lastPeerId
    } else {
      ctx.lastPeerId = peer.id
    }
    console.log('ID: ' + peer.id)
  })
  peer.on('connection', function (c) {
    // Allow only a single connection
    if (ctx.conn && ctx.conn.open) {
      c.on('open', function () {
        c.send('Already connected to another client')
        setTimeout(function () {
          c.close()
        }, 500)
      })
      return
    }
    console.log('Connected to: ' + c.peer)
    ctx.conn = c
    handleMessages(ctx)
  })
  peer.on('disconnected', function () {
    console.log('Connection lost. Please reconnect')
  })
  peer.on('close', function () {
    console.log('Connection destroyed')
    reset(ctx)
  })
  peer.on('error', function (err) {
    console.log(err)
    alert('' + err)
    reset(ctx)
  })
  ctx.peer = peer
}

export default {
  transition: 'page-slide',
  asyncData() {
    return {
      peer: null,
      lastPeerId: null,
      conn: null,
      messages: [],
      receiverId: '',
      sendMsg: '',
    }
  },
  methods: {
    create(event) {
      if (event) {
        createPeer(this)
      }
    },
    connect(event) {
      if (event) {
        joinPeer(this)
      }
    },
    send(event) {
      if (event) {
        sendPeer(this)
      }
    },
  },
  mounted() {
    createPeer(this)
  },
}
</script>
