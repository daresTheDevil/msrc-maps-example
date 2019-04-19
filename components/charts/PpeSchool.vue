<template>
  <div>
    <v-container grid-list-md>
      <v-layout row wrap justify-center>
        <v-flex xs8>
          <v-card color="cyan darken-4" dark>
            <v-card-text>
              <v-layout row align-center>
                <v-flex text-xs-left>
                  <h1 class="subheading">School</h1>
                  <h1 class="headline mt-0 font-weight-bold">
                    Oakdale Elementary
                  </h1>
                </v-flex>
                <v-flex>
                  <h1 class="subheading">Enrollment</h1>
                  <h1 class="headline mt-0 font-weight-bold">456</h1>
                </v-flex>
                <v-flex text-xs-right>
                  <h1 class="subheading">Total PPE</h1>
                  <h1 class="headline mt-0 font-weight-bold">$7,201.93</h1>
                </v-flex>
              </v-layout>
            </v-card-text>
          </v-card>
        </v-flex>
      </v-layout>
    </v-container>
    <v-container grid-list-md>
      <v-layout row wrap justify-center>
        <v-flex xs4>
          <apexchart
            type="pie"
            width="80%"
            :options="chartOptions"
            :series="series"
          />
        </v-flex>
        <v-flex xs6 text-xs-left>
          <h1 class="title">School Level Expenditures</h1>
          <p class="caption">
            School level expenses are spent specifically at a school for the
            benefit of only those students and may include items such as staff
            salaries and benefits, supplies, furniture, or utilities.
          </p>
          <v-list dense>
            <v-list-tile>
              <v-list-tile-avatar size="24" tile color="amber darken-2" />
              <v-list-tile-title>
                State/Local Funds
                <sup>1</sup>
              </v-list-tile-title>
            </v-list-tile>
            <v-list-tile>
              <v-list-tile-avatar size="24" tile color="yellow darken-1" />
              <v-list-tile-title>
                Federal Funds
                <sup>2</sup>
              </v-list-tile-title>
            </v-list-tile>
          </v-list>
          <h1 class="title mt-2">District Level Expenditures</h1>
          <p class="caption">
            District level expenses are spent for the benefit of all students in
            a district and may include items such as central office operations,
            transportation, technology operations, maintenance, or property
            insurance.
          </p>
          <v-list dense>
            <v-list-tile>
              <v-list-tile-avatar size="20" tile color="teal" />
              <v-list-tile-title>
                State/Local Funds
                <sup>1</sup>
              </v-list-tile-title>
            </v-list-tile>
            <v-list-tile>
              <v-list-tile-avatar size="20" tile color="cyan" />
              <v-list-tile-title>
                Federal Funds
                <sup>2</sup>
              </v-list-tile-title>
            </v-list-tile>
          </v-list>
        </v-flex>
      </v-layout>
    </v-container>
    <section class="pt-5 text-xs-left">
      <v-container grid-list-md>
        <v-layout row wrap justify-center>
          <v-flex xs12 md6>
            <v-alert
              value="true"
              color="grey lighten-4 grey--text text--darken-3"
            >
              <h1 class="subheading font-weight-bold">1. State/Local Funds</h1>
              <p>
                State and Local Funds include ad valorem taxes, school fees,
                athletic events, donations, MS Adequate Education Funds,
                Education Enhancement Funds, and other State Grants.
              </p>

              <h1 class="subheading font-weight-bold">2. Federal Funds</h1>
              <p>
                Federal Funds include Title programs, Special Education
                programs, Medicaid or Rehabilitation programs, and other federal
                grants.
              </p>
            </v-alert>
          </v-flex>
        </v-layout>
      </v-container>
    </section>
  </div>
</template>

<script>
import VueApexCharts from 'vue-apexcharts'
export default {
  components: { apexchart: VueApexCharts },
  data() {
    return {
      series: [44, 55, 13, 43],
      chartOptions: {
        labels: ['Team A', 'Team B', 'Team C', 'Team D'],
        legend: {
          show: false
        },
        responsive: [
          {
            breakpoint: 480,
            options: {
              chart: {
                width: 200
              },
              legend: {
                show: false,
                position: 'bottom'
              }
            }
          }
        ]
      }
    }
  }
}
</script>

<style>
.theme--light.v-table thead tr:first-child {
  border-bottom: 0px solid rgba(0, 0, 0, 0.12);
}
</style>
