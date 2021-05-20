<template>
  <v-app id="inspire">
    <v-app-bar dense dark>
      <v-btn icon>
        <v-icon>mdi-view-dashboard</v-icon>
      </v-btn>

      <v-toolbar-title>Administrador de Expedientes</v-toolbar-title>
    </v-app-bar>

    <v-main>
      <v-container>
        <v-row>
          <v-col cols="12" sm="2">
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
