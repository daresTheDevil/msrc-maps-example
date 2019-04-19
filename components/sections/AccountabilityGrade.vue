<template>
  <section v-if="mathFacts.length > 0">
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
            Accountability Grade
          </h1>
          <p :class="$vuetify.breakpoint.mdAndUp ? 'subheading' : 'body-1'">
            The Mississippi Statewide Accountability System is a single "A"
            through "F" school and district accountability system. Click any
            chart to get detailed information on the measure.
          </p>
        </v-flex>
      </v-layout>
      <v-divider class="my-3" />
      <v-layout row wrap align-center>
        <v-flex xs12 md5 class="text-xs-center text-md-left">
          <h1
            :class="$vuetify.breakpoint.mdAndUp ? 'display-1' : 'headline'"
            class="font-weight-bold"
          >
            Math Components
          </h1>
          <p :class="$vuetify.breakpoint.mdAndUp ? 'subheading' : 'body-1'">
            Measurements of student performance on the statewide math
            assessment.
          </p>
        </v-flex>
        <v-flex xs12 md7>
          <v-layout row wrap>
            <v-flex
              v-for="(fact, i) in mathFacts"
              :key="i"
              xs4
              @click="navigate(fact)"
            >
              <gauge-card :color="accountabilityColor" :data="fact" />
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
            English Components
          </h1>
          <p :class="$vuetify.breakpoint.mdAndUp ? 'subheading' : 'body-1'">
            Measurements of student performance on the statewide math
            assessment.
          </p>
        </v-flex>
        <v-flex xs12 md7>
          <v-layout row wrap>
            <v-flex
              v-for="(fact, j) in englishFacts"
              :key="j"
              xs4
              @click="navigate(fact)"
            >
              <gauge-card :color="accountabilityColor" :data="fact" />
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
            Other
          </h1>
          <p :class="$vuetify.breakpoint.mdAndUp ? 'subheading' : 'body-1'">
            Other measurements of student performance that factor into the
            accountability grade.
          </p>
        </v-flex>
        <v-flex xs12 md7>
          <v-layout row wrap>
            <v-flex
              v-for="(fact, k) in otherFacts"
              :key="k"
              xs4
              @click="navigate(fact)"
            >
              <gauge-card :color="accountabilityColor" :data="fact" />
            </v-flex>
          </v-layout>
        </v-flex>
      </v-layout>
    </v-container>
    {{ text }}
  </section>
</template>

<script>
// import { mapGetters } from 'vuex'
import facts from '@/assets/data/facts.json'
import GaugeCard from '@/components/cards/GaugeCard.vue'

export default {
  components: {
    GaugeCard
  },
  data() {
    return {
      facts,
      accountabilityColor: '#2c5875',
      text: ''
    }
  },
  computed: {
    mathFacts() {
      return this.facts.filter(
        fact =>
          (fact.componentCode === 'MP' ||
            fact.componentCode === 'MG' ||
            fact.componentCode === 'MGL25') &&
          fact.entityId === this.$route.params.id &&
          fact.aggregationCode === 'ALL'
      )
    },
    englishFacts() {
      return this.facts.filter(
        fact =>
          (fact.componentCode === 'EP' ||
            fact.componentCode === 'EG' ||
            fact.componentCode === 'EGL25') &&
          fact.entityId === this.$route.params.id &&
          fact.aggregationCode === 'ALL'
      )
    },
    otherFacts() {
      return this.facts.filter(
        fact =>
          (fact.componentCode === 'HP' ||
            fact.componentCode === 'SP' ||
            fact.componentCode === 'CCR' ||
            fact.componentCode === 'ACC' ||
            fact.componentCode === 'GR' ||
            fact.componentCode === 'ELL') &&
          fact.entityId === this.$route.params.id &&
          fact.aggregationCode === 'ALL'
      )
    }
    // ...mapGetters({ mathFacts: 'getLandingMathFacts' }),
    // ...mapGetters({ englishFacts: 'getLandingEnglishFacts' }),
    // ...mapGetters({ otherFacts: 'getLandingOtherFacts' })
  },
  methods: {
    navigate(fact) {
      // eslint-disable-next-line no-console
      console.log('navigate', fact.componentCode)
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
