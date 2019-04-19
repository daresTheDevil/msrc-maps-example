<template>
  <section v-if="participationFacts.length > 0">
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
            Student Assessment Results
          </h1>
          <p :class="$vuetify.breakpoint.mdAndUp ? 'subheading' : 'body-1'">
            The following sections show detailed information on student
            performance and participation on statewide assessments. Click any
            icon to get detailed information on the measure.
          </p>
        </v-flex>
      </v-layout>
      <v-layout row wrap align-center>
        <v-flex xs12 md5 class="text-xs-center text-md-left">
          <h1
            :class="$vuetify.breakpoint.mdAndUp ? 'display-1' : 'headline'"
            class="font-weight-bold"
          >
            Student Performance
          </h1>
          <p :class="$vuetify.breakpoint.mdAndUp ? 'subheading' : 'body-1'">
            Student performance shows detailed information about each level of
            performance on statewide assessments by student subgroups.
          </p>
        </v-flex>
        <v-flex xs12 md7>
          <v-layout row wrap>
            <v-flex
              xs4
              @click="
                navigate({ componentCode: 'mperf', entityId: $route.params.id })
              "
            >
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
                    mdi-table-plus
                  </v-icon>
                </v-card-text>

                <v-card-text class="pt-0">
                  <h1
                    v-if="$vuetify.breakpoint.smAndUp"
                    class="title font-weight-regular text-xs-center"
                  >
                    Math Performance
                  </h1>
                  <h1
                    v-if="$vuetify.breakpoint.smAndDown"
                    class="caption font-weight-regular text-xs-center"
                  >
                    Math Performance
                  </h1>
                </v-card-text>
              </v-card>
            </v-flex>
            <v-flex
              xs4
              @click="
                navigate({ componentCode: 'eperf', entityId: $route.params.id })
              "
            >
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
                    mdi-book-open-page-variant
                  </v-icon>
                </v-card-text>

                <v-card-text class="pt-0">
                  <h1
                    v-if="$vuetify.breakpoint.smAndUp"
                    class="title font-weight-regular text-xs-center"
                  >
                    English Performance
                  </h1>
                  <h1
                    v-if="$vuetify.breakpoint.smAndDown"
                    class="caption font-weight-regular text-xs-center"
                  >
                    English Performance
                  </h1>
                </v-card-text>
              </v-card>
            </v-flex>
            <v-flex
              xs4
              @click="
                navigate({ componentCode: 'sperf', entityId: $route.params.id })
              "
            >
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
                    mdi-atom
                  </v-icon>
                </v-card-text>

                <v-card-text class="pt-0">
                  <h1
                    v-if="$vuetify.breakpoint.smAndUp"
                    class="title font-weight-regular text-xs-center"
                  >
                    Science Performance
                  </h1>
                  <h1
                    v-if="$vuetify.breakpoint.smAndDown"
                    class="caption font-weight-regular text-xs-center"
                  >
                    Science Performance
                  </h1>
                </v-card-text>
              </v-card>
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
            Student Participation
          </h1>
          <p :class="$vuetify.breakpoint.mdAndUp ? 'subheading' : 'body-1'">
            The percent of all students enrolled in a Mississippi public school
            who participated in the yearly statewide assessments.
          </p>
        </v-flex>
        <v-flex xs12 md7>
          <v-layout row wrap>
            <v-flex
              v-for="(fact, i) in participationFacts"
              :key="i"
              xs4
              @click="navigate(fact)"
            >
              <gauge-card :color="assessmentColor" :data="fact" />
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
            Alternate Participation
          </h1>
          <p :class="$vuetify.breakpoint.mdAndUp ? 'subheading' : 'body-1'">
            The percent of students in Mississippi public schools who
            participated in the yearly statewide alternate assessment for
            students with significant cognitive disabilities.
          </p>
        </v-flex>
        <v-flex xs12 md7>
          <v-layout row wrap>
            <v-flex
              v-for="(fact, j) in alternateParticipationFacts"
              :key="j"
              xs4
              @click="navigate(fact)"
            >
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
import facts from '@/assets/data/facts.json'

export default {
  components: {
    GaugeCard
  },
  data() {
    return {
      facts,
      assessmentColor: '#7b335a'
    }
  },
  computed: {
    participationFacts() {
      return this.facts.filter(
        fact =>
          (fact.componentCode === 'MATHPT' ||
            fact.componentCode === 'ENGPT' ||
            fact.componentCode === 'SCIPT') &&
          fact.entityId === this.$route.params.id &&
          fact.aggregationCode === 'ALL'
      )
    },
    alternateParticipationFacts() {
      return this.facts.filter(
        fact =>
          (fact.componentCode === 'MATHALTP' ||
            fact.componentCode === 'ENGALTP' ||
            fact.componentCode === 'SCIALTP') &&
          fact.entityId === this.$route.params.id &&
          fact.aggregationCode === 'ALL'
      )
    }
    // ...mapGetters({ participationFacts: 'getLandingParticipationFacts' }),
    // ...mapGetters({
    //   alternateParticipationFacts: 'getLandingAlternateParticipationFacts'
    // })
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
    }
  }
}
</script>

<style></style>
