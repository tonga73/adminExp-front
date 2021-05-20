<template>
  <v-row>
    <v-col class="text-center">
      <list-record />
    </v-col>
  </v-row>
</template>

<script>
import ListRecord from '~/components/Record/ListRecord.vue'

export default {
  components: {
    ListRecord,
  },
}
</script>
