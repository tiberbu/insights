<!-- eslint-disable no-console -->
<!-- eslint-disable no-console -->
<template>
  <div>
    <h6> {{ title }}</h6>
    <div
      id="my-superset-container"
      style="width:100%; min-height:100vh;"
    />
  </div>
</template>


<script>

export default {
  props: {
    title: { type: String, default: 'LIVE REGISTRATION' },
    dashboardId: { type: String, default: '13' },
    embedId: { type: String, default: '479083ee-c9c1-481f-9369-761bcc72c843' },
  },
  mounted() {
    // Check if the supersetEmbeddedSdk SDK is already defined
    if (typeof window.supersetEmbeddedSdk === 'undefined') {
      // Load the supersetEmbeddedSdk SDK dynamically
      const script = document.createElement('script')
      script.src = 'https://unpkg.com/@superset-ui/embedded-sdk'
      script.onload = () => {
        this.embedSupersetDashboard()
      }
      document.head.appendChild(script)
    } else {
      this.embedSupersetDashboard()
    }
  },
  methods: {
    async embedSupersetDashboard() {
       const message = "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VyIjp7InVzZXJuYW1lIjoiYWRtaW4iLCJmaXJzdF9uYW1lIjoiU3VwZXJzZXQiLCJsYXN0X25hbWUiOiJBZG1pbiJ9LCJyZXNvdXJjZXMiOlt7InR5cGUiOiJkYXNoYm9hcmQiLCJpZCI6IjEzIn1dLCJybHNfcnVsZXMiOltdLCJpYXQiOjE3MTk3MzgxMzEuNTAwMzI3LCJleHAiOjE3MTk3Mzg0MzEuNTAwMzI3LCJhdWQiOiJodHRwOi8vc3VwZXJzZXQ6ODA4OC8iLCJ0eXBlIjoiZ3Vlc3QifQ.p58-7QQWqYK44gW_ja71XVP3yqFhMVyhNI4JDNIyBYA";
      if (typeof window.supersetEmbeddedSdk !== 'undefined') {
        // Access the supersetEmbeddedSdk object from the global scope
       
        const { supersetEmbeddedSdk } = window

        // Your Superset dashboard embed configuration
        const dashboardEmbed = await supersetEmbeddedSdk.embedDashboard({
          id: this.embedId, // given by the Superset embedding UI
          supersetDomain: 'http://3.8.237.9',
          mountPoint: document.getElementById('my-superset-container'), // any html element that can contain an iframe
          fetchGuestToken: () => message,
          dashboardUiConfig: {

            hideTitle: true,
            filters: {
              expanded: false,
            },
          },
        })

        // eslint-disable-next-line no-console
        console.log('Dash', dashboardEmbed)
      } else {
        // eslint-disable-next-line no-console
        console.error('supersetEmbeddedSdk SDK is still not defined.')
      }
    },

  },
}
</script>
<style>
iframe {
    min-height: 85vh !important;
    width: 100% !important;
}
</style>
