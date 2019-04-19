<template>
  <section class="teal lighten-5">
    <v-container grid-list-md>
      <v-layout row wrap>
        <v-flex v-for="fact in facts" :key="fact.componentName" xs12>
          <v-card class="my-1" hover @click="shows = !shows">
            <v-card-text class="pa-0">
              <v-layout row wrap align-center justify-space-between>
                <v-flex shrink class="text-xs-center">
                  <img
                    src="/images/science-growth-icon.svg"
                    height="90"
                    class="pt-2 ml-3"
                  />
                </v-flex>
                <v-flex xs5>
                  <h1
                    class="display-1 font-weight-bold"
                    v-text="fact.componentName"
                  />
                  <h1 class="body-1" v-text="fact.componentShortDescription" />
                </v-flex>
                <v-flex 2 class="text-xs-center">
                  <h1 class="display-3 font-weight-black">
                    {{ fact.componentValue }}%
                  </h1>
                </v-flex>
                <v-flex xs4 class="py-0">
                  <v-progress-linear
                    color="light-blue accent-3"
                    value="65"
                    class="my-0"
                    height="40"
                  >
                    <v-chip
                      small
                      dark
                      class="my-2 white--text subheading"
                      color="light-blue accent-3"
                    >
                      State - 65%
                    </v-chip>
                  </v-progress-linear>
                  <v-progress-linear
                    color="teal accent-3"
                    value="73"
                    class="my-0"
                    height="40"
                  >
                    <v-chip
                      small
                      light
                      class="my-2 subheading"
                      color="teal accent-3"
                    >
                      District - 73%
                    </v-chip>
                  </v-progress-linear>
                  <v-progress-linear
                    color="amber accent-3"
                    :value="fact.componentValue"
                    class="my-0"
                    height="40"
                  >
                    <v-chip
                      small
                      light
                      class="my-2 subheading"
                      color="amber accent-3"
                    >
                      School - {{ fact.componentValue }}%
                    </v-chip>
                  </v-progress-linear>
                </v-flex>
              </v-layout>
            </v-card-text>
            <v-slide-y-transition>
              <v-card-text v-if="shows" class="px-0 pb-0">
                <v-divider />
                <v-sparkline
                  class="pb-0"
                  :value="value"
                  :gradient="gradient"
                  height="50"
                  padding="0"
                  type="trend"
                  fill
                  stroke-linecap="round"
                  smooth
                />
              </v-card-text>
            </v-slide-y-transition>
          </v-card>
        </v-flex>
      </v-layout>
    </v-container>
  </section>
</template>

<script>
import { mapGetters } from 'vuex'
export default {
  data() {
    return {
      value: [423, 446, 675, 510, 590, 610, 760],
      shows: false,
      gradient: ['purple', 'violet'],
      fill: true
    }
  },
  computed: {
    ...mapGetters({ facts: 'getLandingProficiencyFacts' })
  }
}
</script>

<style></style>
