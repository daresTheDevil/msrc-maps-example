<template>
  <div>
    <template v-if="$vuetify.breakpoint.smAndUp">
      <v-data-table :items="desserts" hide-actions class="elevation-1">
        <template slot="headers">
          <tr>
            <th />
            <th />
            <th colspan="2" class="subheading black--text">
              School Level Funding
            </th>
            <th colspan="2" class="subheading black--text">
              District Level Funding
            </th>
            <th />
          </tr>
          <tr>
            <th class="subheading black--text text-xs-left">Name</th>
            <th class="subheading black--text">Enrollment</th>
            <th class="subheading black--text">State/Local</th>
            <th class="subheading black--text">Federal</th>
            <th class="subheading black--text">State/Local</th>
            <th class="subheading black--text">Federal</th>
            <th class="subheading black--text text-xs-right">Total PPE</th>
          </tr>
        </template>
        <template slot="items" slot-scope="props">
          <td class="text-xs-left font-weight-bold subheading">
            {{ props.item.name }}
          </td>
          <td class="text-xs-center">{{ props.item.enrollment }}</td>
          <td class="text-xs-center">{{ props.item.schoolSL }}</td>
          <td class="text-xs-center">{{ props.item.schoolFed }}</td>
          <td class="text-xs-center">{{ props.item.districtSL }}</td>
          <td class="text-xs-center">{{ props.item.districtFed }}</td>
          <td class="text-xs-right">{{ props.item.totalPpe }}</td>
        </template>
      </v-data-table>
    </template>
    <template v-else>
      <v-card v-for="item in desserts" :key="item.name" class="mt-3">
        <v-card-text>
          <v-list dense>
            <v-list-tile>
              <span class="font-weight-bold">Name</span>
              <v-spacer />
              <span>{{ item.name }}</span>
            </v-list-tile>
            <v-list-tile>
              <span class="font-weight-bold">Enrollment</span>
              <v-spacer />
              <span>{{ item.enrollment }}</span>
            </v-list-tile>
            <v-list-tile>
              <span class="font-weight-bold">School Level State/Local</span>
              <v-spacer />
              <span>{{ item.schoolSL }}</span>
            </v-list-tile>
            <v-list-tile>
              <span class="font-weight-bold">School Level Federal</span>
              <v-spacer />
              <span>{{ item.schoolFed }}</span>
            </v-list-tile>
            <v-list-tile>
              <span class="font-weight-bold">District Level State/Local</span>
              <v-spacer />
              <span>{{ item.districtSL }}</span>
            </v-list-tile>
            <v-list-tile>
              <span class="font-weight-bold">District Level Federal</span>
              <v-spacer />
              <span>{{ item.districtFed }}</span>
            </v-list-tile>
            <v-list-tile>
              <span class="font-weight-bold">Total PPE</span>
              <v-spacer />
              <span>{{ item.totalPpe }}</span>
            </v-list-tile>
          </v-list>
        </v-card-text>
      </v-card>
    </template>
    <section class="pt-5">
      <v-container grid-list-md>
        <v-layout row wrap justify-center>
          <v-flex xs12 md6>
            <v-alert
              value="true"
              color="grey lighten-4 grey--text text--darken-1 text-xs-left"
            >
              <h1 class="body-2">Exclusions</h1>
              <h1 class="body-1">
                Funding that is not included in this calculation includes Child
                Nutrition Funds, Debt Service Funds and Capitol Project Funds.
                The total amount of exclusions for this district is
                <span class="font-weight-bold">$18,002,993</span>.
              </h1>
            </v-alert>
          </v-flex>
        </v-layout>
      </v-container>
    </section>
  </div>
</template>

<script>
export default {
  data() {
    return {
      desserts: [
        {
          name: 'Northwest Rankin High School',
          enrollment: 458,
          schoolSL: '$4,622.56',
          schoolFed: '$647.74',
          districtSL: '$1,814.61',
          districtFed: '$116.52',
          totalPpe: '$7,201.43'
        },
        {
          name: 'Northwest Rankin Middle School',
          enrollment: 389,
          schoolSL: '$5,163.69',
          schoolFed: '$457.36',
          districtSL: '$1,814.61',
          districtFed: '$116.52',
          totalPpe: '$7,552.18'
        },
        {
          name: 'Oakdale Elementary School',
          enrollment: 199,
          schoolSL: '$6,584.20',
          schoolFed: '$339.22',
          districtSL: '$1,814.61',
          districtFed: '$116.52',
          totalPpe: '$8,854.55'
        },
        {
          name: 'Flowood Middle School',
          enrollment: 329,
          schoolSL: '$6,559.33',
          schoolFed: '$258.26',
          districtSL: '$1,814.61',
          districtFed: '$116.52',
          totalPpe: '$8,748.72'
        },
        {
          name: 'Vine Street Elementary',
          enrollment: 582,
          schoolSL: '$6,767.68',
          schoolFed: '$359.54',
          districtSL: '$1,814.61',
          districtFed: '$116.52',
          totalPpe: '$9,058.35'
        }
      ]
    }
  }
}
</script>

<style>
.theme--light.v-table thead tr:first-child {
  border-bottom: 0px solid rgba(0, 0, 0, 0.12);
}
</style>
