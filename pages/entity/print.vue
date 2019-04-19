<template>
  <div>
    <v-container grid-list-md>
      <v-layout>
        <v-flex>
          <v-card id="capture" ref="capture" color="white" light>
            <v-toolbar flat color="white" light card>
              <img src="/images/mde-logo-color.svg" height="60" />
              <v-spacer />
              <v-toolbar-title>
                <div class="mt-2">
                  <h1 class="title font-weight-medium">
                    District Report Card 2017 - 2018
                  </h1>
                  <h1 class="subheading font-weight-light">
                    for more detailed information, please visit
                    http://msrc.mdek12.org
                  </h1>
                </div>
              </v-toolbar-title>
              <v-spacer />
              <v-btn white flat @click="print" />
            </v-toolbar>
            <v-card-text class="primary white--text">
              <h1 class="display-2 text-xs-center">
                West Point Consolidated School District
              </h1>
              <h1 class="display-1 text-xs-center">
                Mississippi
              </h1>
              <br />
              <v-layout row wrap justify-center align-center>
                <v-flex shrink class="text-xs-center">
                  <h1 class="subheading">
                    Grade
                  </h1>
                  <h1 style="font-size: 8rem; line-height: 8rem;">
                    C
                  </h1>
                </v-flex>
                <v-flex shrink>
                  <v-list class="transparent" dark>
                    <v-list-tile>
                      <v-list-tile-avatar>
                        <v-icon medium>
                          mdi-map-marker
                        </v-icon>
                      </v-list-tile-avatar>
                      <v-list-tile-content>
                        <v-list-tile-sub-title
                          >359 W Commerce Street</v-list-tile-sub-title
                        >
                        <v-list-tile-sub-title
                          >West Point, MS 39047</v-list-tile-sub-title
                        >
                      </v-list-tile-content>
                    </v-list-tile>
                    <v-divider inset />
                    <v-list-tile>
                      <v-list-tile-avatar>
                        <v-icon medium>
                          mdi-account
                        </v-icon>
                      </v-list-tile-avatar>
                      <v-list-tile-content>
                        <v-list-tile-title>John Doe</v-list-tile-title>
                        <v-list-tile-sub-title>
                          JohnDoe@district.edu
                        </v-list-tile-sub-title>
                      </v-list-tile-content>
                    </v-list-tile>
                  </v-list>
                </v-flex>
              </v-layout>
            </v-card-text>
            <v-card-text class="pr-0">
              <v-layout row>
                <v-flex xs9>
                  <v-layout column>
                    <v-flex>
                      <h1 class="title">
                        School Accountability Grade Components
                      </h1>
                      <p>
                        Mississippi's accountability system assigns "A" through
                        "F" letter grades for schools and districts. Grades are
                        based on student achievement, student growth, student
                        participation in testing, and other academic measures.
                      </p>
                    </v-flex>

                    <v-flex>
                      <h1 class="headline font-weight-medium teal--text mt-3">
                        Math
                      </h1>
                      <v-divider />
                      <span
                        >Measurements of student performance on the statewide
                        math assessment.</span
                      >
                      <v-layout row justify-center>
                        <v-flex v-for="i in 3" :key="i" xs3>
                          <v-sheet color="cyan" height="100" />
                        </v-flex>
                      </v-layout>
                    </v-flex>

                    <v-flex>
                      <h1 class="headline font-weight-medium teal--text mt-3">
                        English
                      </h1>
                      <v-divider />
                      <span
                        >Measurements of student performance on the statewide
                        English language arts (ELA) assessment.</span
                      >
                      <v-layout row justify-center>
                        <v-flex v-for="i in 3" :key="i" xs3>
                          <v-sheet color="cyan" height="100" />
                        </v-flex>
                      </v-layout>
                    </v-flex>

                    <v-flex>
                      <h1 class="headline font-weight-medium teal--text mt-3">
                        Other Measures
                      </h1>
                      <v-divider />
                      <span
                        >Other measurements of student performance that factor
                        into the accountability grade.</span
                      >
                      <v-layout row wrap justify-center>
                        <v-flex v-for="i in 6" :key="i" xs3>
                          <v-sheet color="cyan" height="100" />
                        </v-flex>
                      </v-layout>
                    </v-flex>
                  </v-layout>
                </v-flex>

                <v-flex xs3 class="grey lighten-3">
                  <v-layout column>
                    <v-flex>
                      <h1 class="title font-weight-bold text-xs-center my-3">
                        Teacher Data
                      </h1>
                      <h1 class="display-1 font-weight-bold text-xs-center">
                        220
                      </h1>
                      <h1
                        class="headline font-weight-bold text-xs-center mx-5 light-blue lighten-3"
                      >
                        Teachers
                      </h1>
                    </v-flex>
                    <v-flex class="text-xs-center">
                      <v-icon size="102" color="black" class="mt-3">
                        mdi-teach
                      </v-icon>
                    </v-flex>
                    <v-flex class="text-xs-center">
                      <v-responsive height="80">
                        <v-divider vertical class="black" />
                      </v-responsive>
                    </v-flex>
                    <v-flex>
                      <v-sheet height="100" color="teal" class="mx-5" />
                      <h1 class="title font-weight-regular text-xs-center">
                        Experienced Teachers
                      </h1>
                    </v-flex>
                    <v-flex class="text-xs-center">
                      <v-responsive height="80">
                        <v-divider vertical class="black" />
                      </v-responsive>
                    </v-flex>
                    <v-flex>
                      <v-sheet height="100" color="teal" class="mx-5" />
                      <h1 class="title font-weight-regular text-xs-center">
                        Experienced Teachers
                      </h1>
                    </v-flex>
                    <v-flex class="text-xs-center">
                      <v-responsive height="80">
                        <v-divider vertical class="black" />
                      </v-responsive>
                    </v-flex>
                    <v-flex>
                      <v-sheet height="100" color="teal" class="mx-5" />
                      <h1 class="title font-weight-regular text-xs-center">
                        Experienced Teachers
                      </h1>
                    </v-flex>
                  </v-layout>
                </v-flex>
              </v-layout>
            </v-card-text>
          </v-card>
        </v-flex>
      </v-layout>
    </v-container>
  </div>
</template>

<script>
import html2canvas from 'html2canvas' // converts DIV to image for print
import Jspdf from 'jspdf'

export default {
  layout: 'print',
  methods: {
    print() {
      const doc = new Jspdf('p', 'pt', 'letter', true)
      // doc.fromHTML(document.getElementById('capture'))
      const position = 0
      html2canvas(this.$refs.capture)
        .then(canvas => {
          const width = doc.internal.pageSize.getWidth()
          const height = doc.internal.pageSize.getHeight()
          const ratio = width / height
          // this.printFile = canvas.toDataURL()
          doc.addImage(canvas, 'PNG', 0, position, width, width * ratio * 3)
          doc.save('Spec_Sheet.pdf')
          // window.open(this.printFile)
        })
        .catch(error => {
          alert(error)
        })
      // .then(this.$router.replace("/"));
      // this.$router.replace("/")
      // this.printSnackbar = true
    }
  }
}
</script>

<style></style>
