<template>
  <div>
    <b-row class="justify-content-md-center mt-2">
      <h1>Hello D2D Players! 👋</h1>
    </b-row>
    <b-row class="mt-2">
      <b-button block variant="outline-primary" href="/host">Host</b-button>
    </b-row>
    <b-row class="mt-4">
      <b-button block variant="outline-primary" href="/client">Client</b-button>
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
