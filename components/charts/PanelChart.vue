<template>
  <v-container
    :fluid="$vuetify.breakpoint.mdAndDown"
    :class="$vuetify.breakpoint.mdAndDown ? 'px-0' : ''"
  >
    <v-layout column class="px-0">
      <v-flex v-if="$vuetify.breakpoint.mdAndUp">
        <v-layout row justify-center>
          <v-chip color="amber">
            Level 1 - Minimal
          </v-chip>
          <v-chip color="amber lighten-3">
            Level 2 - Basic
          </v-chip>
          <v-chip color="grey lighten-2">
            Level 3 - Passing
          </v-chip>
          <v-chip color="teal accent-2">
            Level 4 - Proficient
          </v-chip>
          <v-chip color="teal accent-4">
            Level 5 - Advanced
          </v-chip>
        </v-layout>
      </v-flex>
      <v-flex v-else>
        <v-layout row justify-center>
          <v-chip color="amber" class="mx-0">
            <h1 class="caption">Level 1<br />Minimal</h1>
          </v-chip>
          <v-chip color="amber lighten-3" class="mx-0">
            <h1 class="caption">Level 2<br />Basic</h1>
          </v-chip>
          <v-chip color="grey lighten-2" class="mx-0">
            <h1 class="caption">Level 3<br />Passing</h1>
          </v-chip>
          <v-chip color="teal accent-2" class="mx-0">
            <h1 class="caption">Level 4<br />Proficient</h1>
          </v-chip>
          <v-chip color="teal accent-4" class="mx-0">
            <h1 class="caption">Level 5<br />Advanced</h1>
          </v-chip>
        </v-layout>
      </v-flex>
      <v-flex>
        <h1 class="title font-weight-regular text-xs-left">
          Female
        </h1>
        <v-layout row>
          <v-flex
            v-for="(item, i) in 5"
            :key="i"
            xs3
            style="border-left: 2px solid #ccc;"
          >
            <v-progress-linear
              :color="color[i]"
              :value="randomNumber()"
              background-opacity="0"
              height="45"
              class="mt-0 mb-1 layout"
            >
              <h3 class="text-xs-left pl-2 pt-2">{{ value }}%</h3>
            </v-progress-linear>
          </v-flex>
        </v-layout>
      </v-flex>
      <v-flex>
        <h1 class="title font-weight-regular text-xs-left">
          Male
        </h1>
        <v-layout row>
          <v-flex
            v-for="(item, i) in 5"
            :key="i"
            xs3
            style="border-left: 2px solid #ccc;"
          >
            <v-progress-linear
              :color="color[i]"
              :value="randomNumber()"
              background-opacity="0"
              height="45"
              class="mt-0 mb-1 layout"
            >
              <h3 class="text-xs-left pl-2 pt-2">{{ value }}%</h3>
            </v-progress-linear>
          </v-flex>
        </v-layout>
      </v-flex>
      <v-flex>
        <h1 class="title font-weight-regular text-xs-left">
          Black or African American
        </h1>
        <v-layout row>
          <v-flex
            v-for="(item, i) in 5"
            :key="i"
            xs3
            style="border-left: 2px solid #ccc;"
          >
            <v-progress-linear
              :color="color[i]"
              :value="randomNumber()"
              background-opacity="0"
              height="45"
              class="mt-0 mb-1 layout"
            >
              <h3 class="text-xs-left pl-2 pt-2">{{ value }}%</h3>
            </v-progress-linear>
          </v-flex>
        </v-layout>
      </v-flex>
      <v-flex>
        <h1 class="title font-weight-regular text-xs-left">
          White
        </h1>
        <v-layout row>
          <v-flex
            v-for="(item, i) in 5"
            :key="i"
            xs3
            style="border-left: 2px solid #ccc;"
          >
            <v-progress-linear
              :color="color[i]"
              :value="randomNumber()"
              background-opacity="0"
              height="45"
              class="mt-0 mb-1 layout"
            >
              <h3 class="text-xs-left pl-2 pt-2">{{ value }}%</h3>
            </v-progress-linear>
          </v-flex>
        </v-layout>
      </v-flex>
    </v-layout>
    {{ tableFacts }}
  </v-container>
</template>

<script>
// import { mapGetters } from 'vuex'
import allFacts from '@/assets/data/facts.json'
import entities from '@/assets/data/entities.json'

export default {
  data() {
    return {
      values: [75, 85, 63, 68, 72],
      entities,
      allFacts,
      value: '',
      color: [
        'amber',
        'amber lighten-3',
        'grey lighten-2',
        'teal accent-2',
        'teal accent-4'
      ]
    }
  },
  computed: {
    // ...mapGetters({ entity: 'getSingleEntity' }),
    // ...mapGetters({ allFacts: 'getDetailsFacts' }),
    tableFacts() {
      const data = []
      const aggregationCodeList = this.allFacts
        .filter(item => item.entityId === this.$route.params.id)
        .map(value => value.aggregationCode)
      const uniqueCodeList = [...new Set(aggregationCodeList)]

      for (let i = 0; i < uniqueCodeList.length; i++) {
        const facts = this.allFacts
          .filter(item => item.aggregationCode === aggregationCodeList[i])
          .reverse()
          .map(fact => ({
            aggregationName: fact.aggregationName,
            entityDisplayName: fact.entityDisplayName,
            componentValue: fact.componentValue,
            suppressionLevelCode: fact.suppressionLevelCode,
            suppressionLevelName: fact.suppressionLevelName,
            suppressionLevelDescription: fact.suppressionLevelDescription,
            suppressionTextCode: fact.suppressionTextCode
          }))
        data.push(facts)
      }
      // return uniqueCodeList
      return data
    }
  },
  methods: {
    randomNumber() {
      const number = Math.floor(Math.random() * Math.floor(100))
      this.value = number
      return number
    }
  }
}
</script>
