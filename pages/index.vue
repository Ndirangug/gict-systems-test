<template>
  <div>
    <v-tabs v-model="tab" background-color="transparent" color="basil" grow>
      <v-tab> MY FORM </v-tab>
      <v-tab> DATA </v-tab>
    </v-tabs>

    <v-tabs-items v-model="tab">
      <v-tab-item><my-form /> </v-tab-item>
      <v-tab-item><fetched-data /> </v-tab-item>
    </v-tabs-items>
  </div>
</template>

<script>
export default {
  name: 'IndexPage',
  data() {
    return {
      tab: null,
    }
  },
}
</script>
