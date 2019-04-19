<template>
  <div>
    <v-data-iterator
      v-if="$vuetify.breakpoint.smAndDown"
      hide-actions
      :items="tableFacts"
      :pagination.sync="pagination"
      content-tag="v-layout"
      row
      light
      wrap
    >
      <template v-slot:item="props">
        <v-slide-x-transition>
          <v-flex xs12 sm6 md4 lg3>
            <v-card>
              <v-card-title
                ><h3>{{ props.item[0].aggregationName }}</h3></v-card-title
              >
              <v-divider />
              <v-list dense>
                <v-list-tile>
                  <v-list-tile-content v-if="entity.entityType === 'School'">
                    School:
                  </v-list-tile-content>
                  <v-list-tile-content v-if="entity.entityType === 'District'">
                    District:
                  </v-list-tile-content>
                  <v-list-tile-content v-if="entity.entityType === 'State'">
                    State:
                  </v-list-tile-content>
                  <v-list-tile-content class="align-end">
                    {{ props.item[0].componentValue }}%
                  </v-list-tile-content>
                </v-list-tile>
                <v-list-tile>
                  <v-list-tile-content>2018/19 Goal:</v-list-tile-content>
                  <v-list-tile-content class="align-end">
                    {{
                      props.item[0].componentInterimGoal
                        ? props.item[0].componentInterimGoal + '%'
                        : 'NA'
                    }}
                  </v-list-tile-content>
                </v-list-tile>
                <v-list-tile>
                  <v-list-tile-content>2021/22 Goal:</v-list-tile-content>
                  <v-list-tile-content class="align-end">
                    {{
                      props.item[0].componentInterimGoal2
                        ? props.item[0].componentInterimGoal2 + '%'
                        : 'NA'
                    }}
                  </v-list-tile-content>
                </v-list-tile>
                <v-list-tile>
                  <v-list-tile-content>2024/25 Goal:</v-list-tile-content>
                  <v-list-tile-content class="align-end">
                    {{
                      props.item[0].componentLongTermGoal
                        ? props.item[0].componentLongTermGoal + '%'
                        : 'NA'
                    }}
                  </v-list-tile-content>
                </v-list-tile>
                <v-list-tile v-if="entity.entityType === 'School'">
                  <v-list-tile-content>District:</v-list-tile-content>
                  <v-list-tile-content class="align-end">
                    {{ props.item[1].componentValue }}%
                  </v-list-tile-content>
                </v-list-tile>
                <v-list-tile v-if="entity.entityType === 'School'">
                  <v-list-tile-content>State:</v-list-tile-content>
                  <v-list-tile-content class="align-end">
                    {{ props.item[2].componentValue }}%
                  </v-list-tile-content>
                </v-list-tile>
                <v-list-tile v-if="entity.entityType === 'District'">
                  <v-list-tile-content>State:</v-list-tile-content>
                  <v-list-tile-content class="align-end">
                    {{ props.item[1].componentValue }}%
                  </v-list-tile-content>
                </v-list-tile>
              </v-list>
            </v-card>
          </v-flex>
        </v-slide-x-transition>
      </template>
    </v-data-iterator>
    <div
      v-if="$vuetify.breakpoint.smAndDown"
      class="text-xs-center pt-2 justify-space-between"
    >
      <v-btn
        icon
        color="white"
        :disabled="pagination.page === 1"
        @click="pagination.page--"
      >
        <v-icon large color="primary">
          mdi-chevron-left
        </v-icon>
      </v-btn>
      <v-btn
        icon
        color="white"
        :disabled="pagination.page === tableFacts.length"
        @click="pagination.page++"
      >
        <v-icon large color="primary">
          mdi-chevron-right
        </v-icon>
      </v-btn>
    </div>

    <v-data-table
      v-if="$vuetify.breakpoint.smAndUp"
      :items="tableFacts"
      :headers="headers"
      hide-actions
    >
      <template slot="headers">
        <tr>
          <th align="left">
            <span>Group</span>
          </th>
          <th v-if="entity.entityType === 'School'" align="center">
            <span>School</span>
          </th>
          <th v-else-if="entity.entityType === 'District'" align="center">
            <span>District</span>
          </th>
          <th v-else align="center">
            <span>State</span>
          </th>
          <th align="center">
            <span>2018/19 Goal</span>
          </th>
          <th align="center">
            <span>2021/22 Goal</span>
          </th>
          <th align="center">
            <span>2024/25 Goal</span>
          </th>
          <th v-if="entity.entityType === 'School'" align="center">
            <span>District</span>
          </th>
          <th v-if="entity.entityType === 'School'" align="center">
            <span>State</span>
          </th>
          <th v-if="entity.entityType === 'District'" align="center">
            <span>State</span>
          </th>
        </tr>
      </template>
      <template slot="items" slot-scope="props">
        <td class="text-xs-left">
          {{ props.item[0].aggregationName }}
        </td>
        <td>{{ props.item[0].componentValue }}%</td>
        <td>
          {{
            props.item[0].componentInterimGoal
              ? props.item[0].componentInterimGoal + '%'
              : 'NA'
          }}
        </td>
        <td>
          {{
            props.item[0].componentInterimGoal2
              ? props.item[0].componentInterimGoal2 + '%'
              : 'NA'
          }}
        </td>
        <td>
          {{
            props.item[0].componentLongTermGoal
              ? props.item[0].componentLongTermGoal + '%'
              : 'NA'
          }}
        </td>
        <template
          v-if="entity.entityType === 'School'"
          class="layout align-center"
        >
          <td>{{ props.item[1].componentValue }}%</td>
          <td>{{ props.item[2].componentValue }}%</td>
        </template>
        <template v-else-if="entity.entityType === 'District'">
          <td>{{ props.item[1].componentValue }}%</td>
        </template>
      </template>
      <template slot="footer">
        <td :colspan="headers.length">
          <strong>*</strong> Groups with percentages less than 5% are not
          displayed to protect student privacy. See User Guide for more
          information.
        </td>
      </template>
    </v-data-table>
  </div>
</template>

<script>
import { mapGetters } from 'vuex'
export default {
  data() {
    return {
      test: [],
      headers: [
        {
          text: 'Group',
          align: 'left',
          sortable: false,
          value: 'aggregationName',
          width: '28%'
        },
        {
          text: 'District',
          value: 'componentValue',
          sortable: false,
          width: '12%',
          align: 'center'
        },
        {
          text: '2018/19 Goal',
          value: 'componentInterimGoal',
          sortable: false,
          width: '12%',
          align: 'center'
        },
        {
          text: '2021/2022 Goal',
          value: 'componentInterimGoal2',
          sortable: false,
          width: '12%'
        },
        {
          text: '2024/25 Goal',
          value: 'componentLongTermGoal',
          sortable: false,
          width: '12%'
        },
        {
          text: 'State',
          value: 'componentValue',
          sortable: false,
          width: '12%'
        }
      ],
      items: [
        {
          src: 'https://cdn.vuetifyjs.com/images/carousel/squirrel.jpg'
        },
        {
          src: 'https://cdn.vuetifyjs.com/images/carousel/sky.jpg'
        },
        {
          src: 'https://cdn.vuetifyjs.com/images/carousel/bird.jpg'
        },
        {
          src: 'https://cdn.vuetifyjs.com/images/carousel/planet.jpg'
        }
      ],
      pagination: {
        rowsPerPage: 1,
        page: 1
      }
    }
  },
  methods: {},
  computed: {
    pages() {
      if (
        this.pagination.rowsPerPage == null ||
        this.pagination.totalItems == null
      )
        return 0
      return Math.ceil(this.pagination.totalItems / this.pagination.rowsPerPage)
    },
    ...mapGetters({ entity: 'getSingleEntity' }),
    ...mapGetters({ allFacts: 'getDetailsFacts' }),
    tableFacts() {
      const data = []
      const aggregationCodeList = this.allFacts
        .filter(item => item.entityId === this.$route.params.id)
        .map(value => value.aggregationCode)

      for (let i = 0; i < aggregationCodeList.length; i++) {
        const facts = this.allFacts
          .filter(item => item.aggregationCode === aggregationCodeList[i])
          .reverse()
          .map(fact => ({
            aggregationName: fact.aggregationName,
            entityDisplayName: fact.entityDisplayName,
            componentInterimGoal: fact.componentInterimGoal,
            componentInterimGoal2: fact.componentInterimGoal2,
            componentLongTermGoal: fact.componentLongTermGoal,
            componentValue: fact.componentValue
          }))
        data.push(facts)
      }
      return data
    }
  }
}
</script>
