<template>
  <div class="my-form justify-center items-center">
    <form>
      <v-text-field
        v-model="fullNames"
        :counter="20"
        label="Full Names"
        required
        type="text"
      ></v-text-field>
      <v-text-field
        v-model="email"
        label="E-mail"
        required
        type="email"
      ></v-text-field>

      <v-text-field
        v-model="phone"
        label="Phone"
        required
        type="tel"
      ></v-text-field>

      <v-text-field
        v-model="address"
        label="Address"
        required
        type="text"
      ></v-text-field>

      <v-btn :loading="loading" class="mr-4" @click="submit"> submit </v-btn>
    </form>
  </div>
</template>

<script lang="ts">
import Vue from 'vue'

export default Vue.extend({
  name: 'MyForm',

  // @ts-ignore

  data: () => ({
    fullNames: '',
    email: '',
    phone: '',
    address: '',
    errors: [] as string[],
    loading: false,
  }),

  methods: {
    validateName(): boolean {
      if (this.fullNames.length === 0) {
        this.errors.push('fullNames is required')
        this.$toast.error('fullNames is required')

        return false
      }
      if (this.fullNames.length > 20) {
        this.errors.push('fullNames must be less than 20 characters')
        this.$toast.error('fullNames must be less than 20 characters')

        return false
      }
      return true
    },
    validatePhone(): boolean {
      if (this.phone.length === 0) {
        this.errors.push('Phone is required')
        this.$toast.error('Phone is required')
        return false
      }
      if (this.phone.length !== 10) {
        this.errors.push('Phone number invalid. Has to be in 07xxxxxxxx format')
        this.$toast.error(
          'Phone number invalid. Has to be in 07xxxxxxxx format'
        )
        return false
      }
      return true
    },
    validateEmail(): boolean {
      if (this.email.length === 0) {
        this.errors.push('Email is required')
        this.$toast.error('Email is required')
        return false
      }
      if (!this.validEmail(this.email)) {
        this.errors.push('Please enter a valid email adress')
        this.$toast.error('Please enter a valid email adress')
        return false
      }
      return true
    },
    validateAddress(): boolean {
      if (this.address.length === 0) {
        this.errors.push('Addrress is required')
        this.$toast.error('Addrress is required')
        return false
      }

      return true
    },
    validEmail(email: string): boolean {
      const re =
        /^(([^<>()[\]\\.,;:\s@"]+(\.[^<>()[\]\\.,;:\s@"]+)*)|(".+"))@((\[[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\])|(([a-zA-Z\-0-9]+\.)+[a-zA-Z]{2,}))$/
      return re.test(email)
    },

    async submit() {
      this.errors = []
      this.validateName()
      this.validatePhone()
      this.validateEmail()
      this.validateAddress()

      if (this.errors.length === 0) {
        this.loading = true
        try {
          const response = await this.$axios.post(
            'https://developers.gictsystems.com/api/dummy/submit/',
            {
              fullNames: this.fullNames,
              email: this.email,
              phone: this.phone,
              address: this.address,
            }
          )
          console.log('success', response)
          this.$toast.success(response.data.message)
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
        this.loading = false
      } else {
        console.log(this.errors)
      }
    },
  },
})
</script>

