<template>
  <div>
    <v-menu
      :close-on-content-click="false"
      :allow-overflow="true"
      full-width
      offset-y
      max-height="500px"
    >
      <v-text-field
        slot="activator"
        v-model="search"
        label="Search for a single school or district"
        type="text"
        solo
        background-color="grey lighten-2"
        clearable
        flat
        hide-details
        @click:clear="clearSearch"
        @input="filterResults"
      />
      <v-card>
        <v-list light>
          <v-list-tile
            v-for="item in filteredState"
            :key="item.entityDisplayName"
            @click="navigate(item.entityId)"
          >
            <v-list-tile-avatar
              :color="avatarColor(item.entityGrade)"
              class="font-weight-bold white--text headline"
            >
              {{ item.entityGrade }}
            </v-list-tile-avatar>
            <v-list-tile-content>
              <span class="headline">{{ item.entityDisplayName }}</span>
            </v-list-tile-content>
          </v-list-tile>
        </v-list>
        <v-divider v-if="filteredDistricts.length > 0" />
        <v-list light>
          <template v-if="filteredDistricts.length > 0">
            <v-subheader>Districts</v-subheader>
            <v-list-tile
              v-for="item in filteredDistricts"
              :key="item.entityDisplayName"
              @click="navigate(item.entityId)"
            >
              <v-list-tile-avatar
                :color="avatarColor(item.entityGrade)"
                class="font-weight-bold white--text headline"
              >
                {{ item.entityGrade }}
              </v-list-tile-avatar>
              <v-list-tile-content>
                <v-list-tile-title v-text="item.entityDisplayName" />
                <v-list-tile-sub-title v-text="item.entityCity" />
              </v-list-tile-content>
            </v-list-tile>
          </template>
        </v-list>
        <v-divider v-if="filteredSchools.length > 0" />
        <v-list light>
          <template v-if="filteredSchools.length > 0">
            <v-subheader>Schools</v-subheader>
            <v-list-tile
              v-for="item in filteredSchools"
              :key="item.entityDisplayName"
              @click="navigate(item.entityId)"
            >
              <v-list-tile-avatar
                :color="avatarColor(item.entityGrade)"
                class="font-weight-bold white--text headline"
              >
                {{ item.entityGrade }}
              </v-list-tile-avatar>
              <v-list-tile-content>
                <v-list-tile-title v-text="item.entityDisplayName" />
                <v-list-tile-sub-title v-text="item.parentEntityName" />
              </v-list-tile-content>
            </v-list-tile>
          </template>
        </v-list>
      </v-card>
    </v-menu>
  </div>
</template>

<script>
import entities from '@/assets/data/entities.json'

export default {
  data() {
    return {
      search: '',
      entities
    }
  },
  computed: {
    filteredState() {
      return this.entities.filter(item => item.entityType === 'State')
    },
    filteredDistricts() {
      return this.entities.filter(
        item =>
          item.entityType === 'District' &&
          item.entityDisplayName
            .toLowerCase()
            .indexOf(this.search.toLowerCase()) > -1
      )
    },
    filteredSchools() {
      return this.entities.filter(
        item =>
          item.entityType === 'School' &&
          item.entityDisplayName
            .toLowerCase()
            .indexOf(this.search.toLowerCase()) > -1
      )
    }
  },
  methods: {
    avatarColor(grade) {
      if (grade === 'A  ' || grade === 'A*  ') return 'teal darken-2'
      if (grade === 'B  ' || grade === 'B*  ') return 'cyan darken-2'
      if (grade === 'C  ' || grade === 'C*  ') return 'amber darken-2'
      if (grade === 'D  ' || grade === 'D*  ') return 'deep orange darken-1'
      return 'red'
    },
    clearSearch() {
      this.search = ''
    },
    filterResults() {
      this.results = this.entities.filter(
        item =>
          item.entityDisplayName
            .toLowerCase()
            .indexOf(this.search.toLowerCase()) > -1
      )
    },
    navigate(id) {
      this.text = id
      this.snackbar = true
      this.$router.push({
        name: 'entity-id',
        params: { id: id }
      })
    }
  }
}
</script>

<style></style>
