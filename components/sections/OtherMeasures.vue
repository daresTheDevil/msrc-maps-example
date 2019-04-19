<template>
  <section>
    <v-container
      grid-list-sm
      class="my-5"
      :fluid="$vuetify.breakpoint.mdAndDown"
    >
      <v-layout row justify-center>
        <v-flex xs12 md8 class="text-xs-center my-3">
          <h1
            :class="$vuetify.breakpoint.mdAndUp ? 'display-2' : 'display-1'"
            class="font-weight-bold"
          >
            Other Measures
          </h1>
          <p :class="$vuetify.breakpoint.mdAndUp ? 'subheading' : 'body-1'">
            The following sections includes information on the professional
            qualifications of teachers in the State. Click any chart to get
            detailed information on the measure.
          </p>
        </v-flex>
      </v-layout>
      <v-layout row wrap align-center>
        <v-flex xs12 md5 class="text-xs-center text-md-left">
          <h1
            :class="$vuetify.breakpoint.mdAndUp ? 'display-1' : 'headline'"
            class="font-weight-bold"
          >
            Teacher Data
          </h1>
          <p :class="$vuetify.breakpoint.mdAndUp ? 'subheading' : 'body-1'">
            This section includes information on school leaders and the
            professional qulaifications of teachers. All teacher data is
            represented as Full Time Equivalents, or FTEs. For example, a
            teacher who works half a school day would be represented as 0.5
            FTEs.
          </p>
        </v-flex>
        <v-flex xs12 md7>
          <v-layout row wrap align-baseline>
            <v-flex
              v-for="fact in teachingFacts"
              :key="fact.componentCode"
              xs4
              @click="navigateTeacher(fact)"
            >
              <v-card
                v-if="fact.componentCode === 'NUMTCH'"
                flat
                color="transparent"
              >
                <v-card-text class="text-xs-center pb-0">
                  <h1 class="display-1 font-weight-bold">
                    {{ fact.componentValue }}
                  </h1>
                </v-card-text>
                <v-card-text class="pt-0">
                  <h1
                    v-if="$vuetify.breakpoint.smAndUp"
                    class="title font-weight-regular text-xs-center"
                  >
                    {{ fact.componentName }}
                  </h1>
                  <h1
                    v-if="$vuetify.breakpoint.smAndDown"
                    class="caption font-weight-regular text-xs-center"
                  >
                    {{ fact.componentName }}
                  </h1>
                </v-card-text>
              </v-card>
              <gauge-card v-else :color="assessmentColor" :data="fact" />
            </v-flex>
          </v-layout>
        </v-flex>
      </v-layout>
      <v-divider class="my-3" />
      <v-layout row wrap align-center>
        <v-flex xs12 md5 class="text-xs-center text-md-left">
          <h1
            :class="$vuetify.breakpoint.mdAndUp ? 'display-1' : 'headline'"
            class="font-weight-bold"
          >
            Other Measures
          </h1>
          <p :class="$vuetify.breakpoint.mdAndUp ? 'subheading' : 'body-1'">
            This section contains information on teachers and school leaders,
            absenteeism, discipline, advanced coursework, and college/university
            enrollment. Click any chart to get detailed information on the
            measure.
          </p>
        </v-flex>
        <v-flex xs12 md7>
          <v-layout row wrap>
            <v-flex xs4>
              <v-card hover flat color="transparent">
                <v-card-text class="pb-0 text-xs-center">
                  <v-icon
                    :style="
                      $vuetify.breakpoint.mdAndUp
                        ? 'font-size: 7rem; line-height: 7rem;'
                        : 'font-size: 5rem; line-height: 5rem;'
                    "
                    :color="assessmentColor"
                  >
                    mdi-city-variant
                  </v-icon>
                </v-card-text>

                <v-card-text class="pt-0">
                  <h1
                    v-if="$vuetify.breakpoint.smAndUp"
                    class="title font-weight-regular text-xs-center"
                  >
                    Discipline
                  </h1>
                  <h1
                    v-if="$vuetify.breakpoint.smAndDown"
                    class="caption font-weight-regular text-xs-center"
                  >
                    Discipline
                  </h1>
                </v-card-text>
              </v-card>
            </v-flex>
            <v-flex xs4 @click="navigate('perpexp')">
              <v-card hover flat color="transparent">
                <v-card-text class="pb-0 text-xs-center">
                  <v-icon
                    :style="
                      $vuetify.breakpoint.mdAndUp
                        ? 'font-size: 7rem; line-height: 7rem;'
                        : 'font-size: 5rem; line-height: 5rem;'
                    "
                    :color="assessmentColor"
                  >
                    mdi-currency-usd
                  </v-icon>
                </v-card-text>

                <v-card-text class="pt-0">
                  <h1
                    v-if="$vuetify.breakpoint.smAndUp"
                    class="title font-weight-regular text-xs-center"
                  >
                    PPE
                  </h1>
                  <h1
                    v-if="$vuetify.breakpoint.smAndDown"
                    class="caption font-weight-regular text-xs-center"
                  >
                    PPE
                  </h1>
                </v-card-text>
              </v-card>
            </v-flex>
            <v-flex xs4>
              <v-card hover flat color="transparent">
                <v-card-text class="pb-0 text-xs-center">
                  <v-icon
                    :style="
                      $vuetify.breakpoint.mdAndUp
                        ? 'font-size: 7rem; line-height: 7rem;'
                        : 'font-size: 5rem; line-height: 5rem;'
                    "
                    :color="assessmentColor"
                  >
                    mdi-star-outline
                  </v-icon>
                </v-card-text>

                <v-card-text class="pt-0">
                  <h1
                    v-if="$vuetify.breakpoint.smAndUp"
                    class="title font-weight-regular text-xs-center"
                  >
                    NAEP
                  </h1>
                  <h1
                    v-if="$vuetify.breakpoint.smAndDown"
                    class="caption font-weight-regular text-xs-center"
                  >
                    NAEP
                  </h1>
                </v-card-text>
              </v-card>
            </v-flex>
            <v-flex v-for="fact in otherFacts" :key="fact.componentCode" xs4>
              <gauge-card :color="assessmentColor" :data="fact" />
            </v-flex>
          </v-layout>
        </v-flex>
      </v-layout>
    </v-container>
  </section>
</template>

<script>
// import { mapGetters } from 'vuex'
import GaugeCard from '@/components/cards/GaugeCard.vue'

export default {
  components: {
    GaugeCard
  },
  data() {
    return {
      assessmentColor: '#7b335a'
    }
  },
  computed: {
    // ...mapGetters({ teachingFacts: 'getLandingTeachingFacts' }),
    // ...mapGetters({ otherFacts: 'getOtherFacts' })
  },
  methods: {
    navigate(fact) {
      this.$router.push({
        name: 'details-component-id',
        params: {
          component: fact.componentCode.toLowerCase(),
          id: fact.entityId
        }
      })
    },
    navigateTeacher(fact) {
      this.$router.push({
        name: 'details-teacher-id',
        params: {
          id: fact.entityId
        }
      })
    }
  }
}
</script>

<style></style>
