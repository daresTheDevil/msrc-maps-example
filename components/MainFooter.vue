<template>
  <v-footer dark height="auto">
    <v-card flat tile class="grey darken-3 white--text flex">
      <v-card-text class="text-xs-center grey darken-2 py-0">
        <v-btn v-for="icon in icons" :key="icon" class="mx-3 white--text" icon>
          <v-icon size="32px">
            {{ icon }}
          </v-icon>
        </v-btn>
      </v-card-text>

      <v-card-text class="white--text py-0">
        <v-container fluid grid-list-md>
          <v-layout row wrap justify-center align-center>
            <v-flex xs12 md3>
              <h1 class="subheading font-weight-bold">
                Mississippi Department of Education
              </h1>
              <h1 class="body-1">
                P.O. Box 771
              </h1>
              <h1 class="body-1">
                Jackson, MS 39205-0771
              </h1>
              <h1 class="body-1">
                Educator Licensure Related Information: 601-359-3483
              </h1>
              <h1 class="body-1">
                General MDE Related Information: 601-359-3513
              </h1>
            </v-flex>
            <v-flex shrink>
              <img src="/images/mde-logo-light.svg" width="130" /><br
                v-if="$vuetify.breakpoint.mdAndUp"
              />
              <img src="/images/msGov.png" width="130" />
            </v-flex>
          </v-layout>
        </v-container>
      </v-card-text>

      <v-divider />

      <v-card-text class="white--text text-xs-center">
        Last Updated 03/28/2019
      </v-card-text>
    </v-card>
  </v-footer>
</template>

<script>
export default {
  data: () => ({
    icons: ['mdi-facebook', 'mdi-twitter', 'mdi-youtube']
  })
}
</script>
