<template>
  <v-app light>
    <v-navigation-drawer v-model="drawer" fixed app>
      <v-list class="transparent">
        <v-list-tile
          v-for="(item, i) in links"
          :key="i"
          active-class="grey lighten-3"
          :to="item.to"
          router
          exact
        >
          <v-list-tile-avatar>
            <v-icon>{{ item.icon }}</v-icon>
          </v-list-tile-avatar>
          <v-list-tile-content>
            <v-list-tile-title v-text="item.text" />
          </v-list-tile-content>
        </v-list-tile>
      </v-list>
    </v-navigation-drawer>

    <v-toolbar color="primary" dark>
      <a href="/">
        <img src="/images/mde-logo-light.svg" height="50" href="/" />
      </a>
      <v-toolbar-title v-if="$vuetify.breakpoint.mdAndUp">
        <div class="mt-2">
          <h1 class="title font-weight-medium">
            Mississippi Department of Education
          </h1>
          <h1 class="subheading font-weight-light">
            Mississippi Succeeds Report Card
          </h1>
        </div>
      </v-toolbar-title>
      <v-spacer />
      <v-menu v-if="$vuetify.breakpoint.mdAndUp" offset-y left>
        <v-btn slot="activator" icon>
          <v-icon>mdi-apps</v-icon>
        </v-btn>
        <v-list>
          <v-list-tile v-for="link in links" :key="link.text" :to="link.to">
            <v-list-tile-avatar
              ><v-icon>{{ link.icon }}</v-icon></v-list-tile-avatar
            >
            <v-list-tile-title v-text="link.text" />
          </v-list-tile>
        </v-list>
      </v-menu>

      <v-menu offset-y left>
        <v-btn slot="activator" icon>
          <v-icon>
            mdi-earth
          </v-icon>
        </v-btn>
        <v-list>
          <v-list-tile>
            <v-list-tile-action>
              <img src="https://img.icons8.com/color/48/000000/usa.png" />
            </v-list-tile-action>
            <v-list-tile-content>
              <v-list-tile-title>English</v-list-tile-title>
            </v-list-tile-content>
          </v-list-tile>
          <v-list-tile>
            <v-list-tile-action>
              <img src="https://img.icons8.com/color/48/000000/mexico.png" />
            </v-list-tile-action>
            <v-list-tile-content>
              <v-list-tile-title>Espanol</v-list-tile-title>
            </v-list-tile-content>
          </v-list-tile>
          <v-list-tile>
            <v-list-tile-action>
              <img src="https://img.icons8.com/color/48/000000/china.png" />
            </v-list-tile-action>
            <v-list-tile-content>
              <v-list-tile-title>Chinese</v-list-tile-title>
            </v-list-tile-content>
          </v-list-tile>
          <v-list-tile>
            <v-list-tile-action>
              <img src="https://img.icons8.com/color/48/000000/france.png" />
            </v-list-tile-action>
            <v-list-tile-content>
              <v-list-tile-title>French</v-list-tile-title>
            </v-list-tile-content>
          </v-list-tile>
        </v-list>
      </v-menu>
      <v-btn icon>
        <v-icon>
          mdi-human
        </v-icon>
      </v-btn>
      <v-toolbar-side-icon
        v-if="$vuetify.breakpoint.mdAndDown"
        @click="drawer = !drawer"
      />
    </v-toolbar>
    <v-content>
      <nuxt />
    </v-content>
    <main-footer />
    <v-bottom-sheet v-model="sheet">
      <v-card height="150" class="blue-grey darken-4 white--text" dark>
        <v-container grid-list-md>
          <v-layout row wrap justify-center align-center>
            <v-flex xs8>
              <p>
                The Mississippi Department of Education uses cookies and similar
                technologies to help deliver the MSRC platform. We collect your
                <v-tooltip top color="primary" width="500">
                  <template v-slot:activator="{ on }">
                    <span class="green--text text--lighten-2" v-on="on"
                      >device type, browser, and IP address</span
                    >
                  </template>
                  <v-card flat color="transparent" max-width="600">
                    <v-list three-line class="transparent">
                      <v-list-tile>
                        <v-list-tile-content>
                          <v-list-tile-title>Device type:</v-list-tile-title>
                          <v-list-tile-sub-title
                            >Device type helps us see the ratio of mobile to
                            desktop users viewing MSRC.</v-list-tile-sub-title
                          >
                        </v-list-tile-content>
                      </v-list-tile>

                      <v-list-tile>
                        <v-list-tile-content>
                          <v-list-tile-title>Browser type:</v-list-tile-title>
                          <v-list-tile-sub-title>
                            Browsers (Chrome, Firefox, Edge) implement features
                            in different ways, so we track browsers to deliver
                            features across all browsers.
                          </v-list-tile-sub-title>
                        </v-list-tile-content>
                      </v-list-tile>
                    </v-list>
                  </v-card>
                </v-tooltip>
                to deliver a great experience regardless of device, browser or
                ability. Our privacy policy explains how and why we use these
                technologies and how you can opt-out of sharing this
                information.
              </p>
            </v-flex>
            <v-flex shrink>
              <v-btn depressed color="green" dark @click="sheet = false">
                <v-icon left>
                  mdi-check
                </v-icon>
                OK!
              </v-btn>
              <br />
              <v-btn
                outline
                color="amber lighten-2"
                light
                @click="sheet = false"
              >
                <v-icon left>
                  mdi-comment-question-outline
                </v-icon>
                Tell me more
              </v-btn>
            </v-flex>
          </v-layout>
        </v-container>
      </v-card>
    </v-bottom-sheet>
    <fab />
  </v-app>
</template>

<script>
import Fab from '@/components/Fab.vue'
import MainFooter from '@/components/MainFooter.vue'
export default {
  components: {
    Fab,
    MainFooter
  },
  data() {
    return {
      clipped: false,
      drawer: false,
      sheet: false,
      fixed: false,
      links: [
        {
          icon: 'mdi-playlist-star',
          text: 'Home',
          to: '/'
        },
        {
          icon: 'mdi-map',
          text: 'Map your data',
          to: '/map'
        },
        {
          icon: 'mdi-table-search',
          text: 'Search group data',
          to: '/groupdata'
        }
      ]
    }
  },
  watch: {},
  mounted() {}
}
</script>
