<template>
  <v-app id="inspire">
    <v-main>
      <v-container>
        <v-row>
          <v-col cols="12" sm="2">
            <img src="@/assets/img/logo.png" contain width="173.5" />
            <v-sheet rounded="lg" min-height="268">
              <!--  -->
            </v-sheet>
          </v-col>

          <v-col cols="12" sm="10">
            <v-sheet min-height="70vh" rounded="lg">
              <v-main class="pa-0 rounded">
                <v-container>
                  <nuxt />
                </v-container>
              </v-main>
            </v-sheet>
          </v-col>
        </v-row>
      </v-container>
    </v-main>
  </v-app>
</template>

<script>
export default {
  data: () => ({
    links: ['Lista'],
  }),
}
</script>
