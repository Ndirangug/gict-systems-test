<template>
  <div class="data d-flex justify-center items-center"></div>
</template>

<script lang="ts">
import Vue from 'vue'
console.log('data component')
export default Vue.extend({
  mounted() {
    this.fetchData()
    setInterval(() => {
      this.fetchData()
    }, 10000)
  },
  methods: {
    fetchData() {
      try {
        const response = this.$axios.get(
          'https://developers.gictsystems.com/api/dummy/items/',
          {
            headers: {
              Authorization: 'Bearer ALDJAK23423JKSLAJAF23423J23SAD3',
            },
          }
        )
        console.log('response', response)
      } catch (error: any) {
        if (error.response) {
          // The client was given an error response (5xx, 4xx)
          console.log(error.response)
          this.$toast.error(error.response.data.message)
        } else {
          console.log(error)
          this.$toast.error(error)
        }
      }
    },
  },
})
</script>
