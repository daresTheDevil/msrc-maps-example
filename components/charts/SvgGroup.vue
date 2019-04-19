<template>
  <div>
    <v-layout row justify-center>
      <v-chip color="amber">
        State
      </v-chip>
      <v-chip color="teal accent-3">
        District
      </v-chip>
      <v-chip color="light-blue accent-2">
        School
      </v-chip>
      <v-chip color="grey darken-1 white--text">
        2025 Goal - {{ allFacts[0].componentLongTermGoal }}%
      </v-chip>
    </v-layout>
    <div class="my-3">
      <h1 class="title font-weight-regular text-xs-left">
        Female
      </h1>
      <v-tooltip top color="grey darken-4">
        <div v-for="(item, fe) in femaleFacts" :key="fe" slot="activator">
          <v-progress-linear
            slot="activator"
            :value="item.componentValue"
            :buffer-value="bufferValue(item)"
            :color="color[fe]"
            height="45"
            :background-color="
              item.componentLongTermGoal ? 'grey' : 'grey lighten-2'
            "
            class="grey lighten-2 text-xs-left my-1"
            buffer
          >
            <v-layout justify-end>
              <h1 class="title font-weight-regular pa-3">
                {{ item.componentValue }}%
              </h1>
            </v-layout>
          </v-progress-linear>
        </div>
        <v-card color="transparent" dark>
          <v-card-text class="title font-weight-regular pb-0">
            Female
          </v-card-text>
          <v-card-text class="pt-0">
            <v-list class="transparent">
              <v-list-tile v-for="(item, fem) in femaleFacts" :key="fem">
                <v-list-tile-avatar>
                  <v-chip :color="color[fem]" class="black--text">
                    {{ item.componentValue }}%
                  </v-chip>
                </v-list-tile-avatar>
                <v-list-tile-title>{{
                  item.entityDisplayName
                }}</v-list-tile-title>
              </v-list-tile>
            </v-list>
          </v-card-text>
        </v-card>
      </v-tooltip>
    </div>
    <div class="my-3">
      <h1 class="title font-weight-regular text-xs-left">
        Male
      </h1>
      <v-tooltip top color="grey darken-4">
        <div v-for="(item, ma) in maleFacts" :key="ma" slot="activator">
          <v-progress-linear
            slot="activator"
            :value="item.componentValue"
            :buffer-value="bufferValue(item)"
            :color="color[ma]"
            height="45"
            :background-color="
              item.componentLongTermGoal ? 'grey' : 'grey lighten-2'
            "
            class="grey lighten-2 text-xs-left my-1"
            buffer
          >
            <v-layout justify-end>
              <h1 class="title font-weight-regular pa-3">
                {{ item.componentValue }}%
              </h1>
            </v-layout>
          </v-progress-linear>
        </div>
        <v-card color="transparent" dark>
          <v-card-text class="title font-weight-regular pb-0">
            Male
          </v-card-text>
          <v-card-text class="pt-0">
            <v-list class="transparent">
              <v-list-tile v-for="(item, mal) in maleFacts" :key="mal">
                <v-list-tile-avatar>
                  <v-chip :color="color[mal]" class="black--text">
                    {{ item.componentValue }}%
                  </v-chip>
                </v-list-tile-avatar>
                <v-list-tile-title>{{
                  item.entityDisplayName
                }}</v-list-tile-title>
              </v-list-tile>
            </v-list>
          </v-card-text>
        </v-card>
      </v-tooltip>
    </div>

    <v-divider class="my-5" />

    <div
      v-if="
        (entity.entityType === 'District' && blFacts.length === 2) ||
          (entity.entityType === 'School' && blFacts.length === 3)
      "
      class="my-3"
    >
      <h1 class="title font-weight-regular text-xs-left">
        Black or African American
      </h1>
      <v-tooltip top color="grey darken-4">
        <div v-for="(item, b) in blFacts" :key="b" slot="activator">
          <v-progress-linear
            slot="activator"
            :value="item.componentValue"
            :buffer-value="bufferValue(item)"
            :color="color[b]"
            height="45"
            :background-color="
              item.componentLongTermGoal ? 'grey' : 'grey lighten-2'
            "
            class="grey lighten-2 text-xs-left my-1"
            buffer
          >
            <v-layout justify-end>
              <h1 class="title font-weight-regular pa-3">
                {{ item.componentValue }}%
              </h1>
            </v-layout>
          </v-progress-linear>
        </div>
        <v-card color="transparent" dark>
          <v-card-text class="title font-weight-regular pb-0">
            Black or African American
          </v-card-text>
          <v-card-text class="pt-0">
            <v-list class="transparent">
              <v-list-tile v-for="(item, bl) in blFacts" :key="bl">
                <v-list-tile-avatar>
                  <v-chip :color="color[bl]" class="black--text">
                    {{ item.componentValue }}%
                  </v-chip>
                </v-list-tile-avatar>
                <v-list-tile-title>{{
                  item.entityDisplayName
                }}</v-list-tile-title>
              </v-list-tile>
            </v-list>
          </v-card-text>
        </v-card>
      </v-tooltip>
    </div>

    <div
      v-if="
        (entity.entityType === 'District' && wFacts.length === 2) ||
          (entity.entityType === 'School' && wFacts.length === 3)
      "
      class="my-3"
    >
      <h1 class="title font-weight-regular text-xs-left">
        White
      </h1>
      <v-tooltip top color="grey darken-4">
        <div v-for="(item, w) in wFacts" :key="w" slot="activator">
          <v-progress-linear
            slot="activator"
            :value="item.componentValue"
            :buffer-value="bufferValue(item)"
            :color="color[w]"
            height="45"
            :background-color="
              item.componentLongTermGoal ? 'grey' : 'grey lighten-2'
            "
            class="grey lighten-2 text-xs-left my-1"
            buffer
          >
            <v-layout justify-end>
              <h1 class="title font-weight-regular pa-3">
                {{ item.componentValue }}%
              </h1>
            </v-layout>
          </v-progress-linear>
        </div>
        <v-card color="transparent" dark>
          <v-card-text class="title font-weight-regular pb-0">
            White
          </v-card-text>
          <v-card-text class="pt-0">
            <v-list class="transparent">
              <v-list-tile v-for="(item, wh) in wFacts" :key="wh">
                <v-list-tile-avatar>
                  <v-chip :color="color[wh]" class="black--text">
                    {{ item.componentValue }}%
                  </v-chip>
                </v-list-tile-avatar>
                <v-list-tile-title>{{
                  item.entityDisplayName
                }}</v-list-tile-title>
              </v-list-tile>
            </v-list>
          </v-card-text>
        </v-card>
      </v-tooltip>
    </div>

    <div
      v-if="
        (entity.entityType === 'District' && naFacts.length === 2) ||
          (entity.entityType === 'School' && naFacts.length === 3)
      "
      class="my-3"
    >
      <h1 class="title font-weight-regular text-xs-left">
        Alaskan or Native American
      </h1>
      <v-tooltip top color="grey darken-4">
        <div v-for="(item, na) in naFacts" :key="na" slot="activator">
          <v-progress-linear
            slot="activator"
            :value="item.componentValue"
            :buffer-value="bufferValue(item)"
            :color="color[na]"
            height="45"
            :background-color="
              item.componentLongTermGoal ? 'grey' : 'grey lighten-2'
            "
            class="grey lighten-2 text-xs-left my-1"
            buffer
          >
            <v-layout justify-end>
              <h1 class="title font-weight-regular pa-3">
                {{ item.componentValue }}%
              </h1>
            </v-layout>
          </v-progress-linear>
        </div>
        <v-card color="transparent" dark>
          <v-card-text class="title font-weight-regular pb-0">
            Alaskan or Native American
          </v-card-text>
          <v-card-text class="pt-0">
            <v-list class="transparent">
              <v-list-tile v-for="(item, nat) in naFacts" :key="nat">
                <v-list-tile-avatar>
                  <v-chip :color="color[nat]" class="black--text">
                    {{ item.componentValue }}%
                  </v-chip>
                </v-list-tile-avatar>
                <v-list-tile-title>{{
                  item.entityDisplayName
                }}</v-list-tile-title>
              </v-list-tile>
            </v-list>
          </v-card-text>
        </v-card>
      </v-tooltip>
    </div>

    <div
      v-if="
        (entity.entityType === 'District' && asFacts.length === 2) ||
          (entity.entityType === 'School' && asFacts.length === 3)
      "
      class="my-3"
    >
      <h1 class="title font-weight-regular text-xs-left">
        Asian
      </h1>
      <v-tooltip top color="grey darken-4">
        <div v-for="(item, as) in asFacts" :key="as" slot="activator">
          <v-progress-linear
            slot="activator"
            :value="item.componentValue"
            :buffer-value="bufferValue(item)"
            :color="color[as]"
            height="45"
            :background-color="
              item.componentLongTermGoal ? 'grey' : 'grey lighten-2'
            "
            class="grey lighten-2 text-xs-left my-1"
            buffer
          >
            <v-layout justify-end>
              <h1 class="title font-weight-regular pa-3">
                {{ item.componentValue }}%
              </h1>
            </v-layout>
          </v-progress-linear>
        </div>
        <v-card color="transparent" dark>
          <v-card-text class="title font-weight-regular pb-0">
            Asian
          </v-card-text>
          <v-card-text class="pt-0">
            <v-list class="transparent">
              <v-list-tile v-for="(item, asn) in asFacts" :key="asn">
                <v-list-tile-avatar>
                  <v-chip :color="color[asn]" class="black--text">
                    {{ item.componentValue }}%
                  </v-chip>
                </v-list-tile-avatar>
                <v-list-tile-title>{{
                  item.entityDisplayName
                }}</v-list-tile-title>
              </v-list-tile>
            </v-list>
          </v-card-text>
        </v-card>
      </v-tooltip>
    </div>

    <div
      v-if="
        (entity.entityType === 'District' && hFacts.length === 2) ||
          (entity.entityType === 'School' && hFacts.length === 3)
      "
      class="my-3"
    >
      <h1 class="title font-weight-regular text-xs-left">
        Hispanic or Latino
      </h1>
      <v-tooltip top color="grey darken-4">
        <div v-for="(item, hi) in hFacts" :key="hi" slot="activator">
          <v-progress-linear
            slot="activator"
            :value="item.componentValue"
            :buffer-value="bufferValue(item)"
            :color="color[hi]"
            height="45"
            :background-color="
              item.componentLongTermGoal ? 'grey' : 'grey lighten-2'
            "
            class="grey lighten-2 text-xs-left my-1"
            buffer
          >
            <v-layout justify-end>
              <h1 class="title font-weight-regular pa-3">
                {{ item.componentValue }}%
              </h1>
            </v-layout>
          </v-progress-linear>
        </div>
        <v-card color="transparent" dark>
          <v-card-text class="title font-weight-regular pb-0">
            Hispanic or Latino
          </v-card-text>
          <v-card-text class="pt-0">
            <v-list class="transparent">
              <v-list-tile v-for="(item, his) in hFacts" :key="his">
                <v-list-tile-avatar>
                  <v-chip :color="color[his]" class="black--text">
                    {{ item.componentValue }}%
                  </v-chip>
                </v-list-tile-avatar>
                <v-list-tile-title>{{
                  item.entityDisplayName
                }}</v-list-tile-title>
              </v-list-tile>
            </v-list>
          </v-card-text>
        </v-card>
      </v-tooltip>
    </div>

    <div
      v-if="
        (entity.entityType === 'District' && tmFacts.length === 2) ||
          (entity.entityType === 'School' && tmFacts.length === 3)
      "
      class="my-3"
    >
      <h1 class="title font-weight-regular text-xs-left">
        Two or More Races
      </h1>
      <v-tooltip top color="grey darken-4">
        <div v-for="(item, tm) in tmFacts" :key="tm" slot="activator">
          <v-progress-linear
            slot="activator"
            :value="item.componentValue"
            :buffer-value="bufferValue(item)"
            :color="color[tm]"
            height="45"
            :background-color="
              item.componentLongTermGoal ? 'grey' : 'grey lighten-2'
            "
            class="grey lighten-2 text-xs-left my-1"
            buffer
          >
            <v-layout justify-end>
              <h1 class="title font-weight-regular pa-3">
                {{ item.componentValue }}%
              </h1>
            </v-layout>
          </v-progress-linear>
        </div>
        <v-card color="transparent" dark>
          <v-card-text class="title font-weight-regular pb-0">
            Two or More Races
          </v-card-text>
          <v-card-text class="pt-0">
            <v-list class="transparent">
              <v-list-tile v-for="(item, tmore) in tmFacts" :key="tmore">
                <v-list-tile-avatar>
                  <v-chip :color="color[tmore]" class="black--text">
                    {{ item.componentValue }}%
                  </v-chip>
                </v-list-tile-avatar>
                <v-list-tile-title>{{
                  item.entityDisplayName
                }}</v-list-tile-title>
              </v-list-tile>
            </v-list>
          </v-card-text>
        </v-card>
      </v-tooltip>
    </div>

    <v-divider class="my-5" />

    <div
      v-if="
        (entity.entityType === 'District' && edisFacts.length === 2) ||
          (entity.entityType === 'School' && edisFacts.length === 3)
      "
      class="my-3"
    >
      <h1 class="title font-weight-regular text-xs-left">
        Economically Disadvantaged
      </h1>
      <v-tooltip top color="grey darken-4">
        <div v-for="(item, ed) in edisFacts" :key="ed" slot="activator">
          <v-progress-linear
            slot="activator"
            :value="item.componentValue"
            :buffer-value="bufferValue(item)"
            :color="color[ed]"
            height="45"
            :background-color="
              item.componentLongTermGoal ? 'grey' : 'grey lighten-2'
            "
            class="grey lighten-2 text-xs-left my-1"
            buffer
          >
            <v-layout justify-end>
              <h1 class="title font-weight-regular pa-3">
                {{ item.componentValue }}%
              </h1>
            </v-layout>
          </v-progress-linear>
        </div>
        <v-card color="transparent" dark>
          <v-card-text class="title font-weight-regular pb-0">
            Economically Disadvantaged
          </v-card-text>
          <v-card-text class="pt-0">
            <v-list class="transparent">
              <v-list-tile v-for="(item, is) in edisFacts" :key="is">
                <v-list-tile-avatar>
                  <v-chip :color="color[is]" class="black--text">
                    {{ item.componentValue }}%
                  </v-chip>
                </v-list-tile-avatar>
                <v-list-tile-title>{{
                  item.entityDisplayName
                }}</v-list-tile-title>
              </v-list-tile>
            </v-list>
          </v-card-text>
        </v-card>
      </v-tooltip>
    </div>

    <div
      v-if="
        (entity.entityType === 'District' && nedisFacts.length === 2) ||
          (entity.entityType === 'School' && nedisFacts.length === 3)
      "
      class="my-3"
    >
      <h1 class="title font-weight-regular text-xs-left">
        Non-economically Disadvantaged
      </h1>
      <v-tooltip top color="grey darken-4">
        <div v-for="(item, ne) in nedisFacts" :key="ne" slot="activator">
          <v-progress-linear
            slot="activator"
            :value="item.componentValue"
            :buffer-value="bufferValue(item)"
            :color="color[ne]"
            height="45"
            :background-color="
              item.componentLongTermGoal ? 'grey' : 'grey lighten-2'
            "
            class="grey lighten-2 text-xs-left my-1"
            buffer
          >
            <v-layout justify-end>
              <h1 class="title font-weight-regular pa-3">
                {{ item.componentValue }}%
              </h1>
            </v-layout>
          </v-progress-linear>
        </div>
        <v-card color="transparent" dark>
          <v-card-text class="title font-weight-regular pb-0">
            Non-econonmically Disadvantaged
          </v-card-text>
          <v-card-text class="pt-0">
            <v-list class="transparent">
              <v-list-tile v-for="(item, nedis) in nedisFacts" :key="nedis">
                <v-list-tile-avatar>
                  <v-chip :color="color[nedis]" class="black--text">
                    {{ item.componentValue }}%
                  </v-chip>
                </v-list-tile-avatar>
                <v-list-tile-title>{{
                  item.entityDisplayName
                }}</v-list-tile-title>
              </v-list-tile>
            </v-list>
          </v-card-text>
        </v-card>
      </v-tooltip>
    </div>

    <div
      v-if="
        (entity.entityType === 'District' && homlsFacts.length === 2) ||
          (entity.entityType === 'School' && homlsFacts.length === 3)
      "
      class="my-3"
    >
      <h1 class="title font-weight-regular text-xs-left">
        Homeless
      </h1>
      <v-tooltip top color="grey darken-4">
        <div v-for="(item, hom) in homlsFacts" :key="hom" slot="activator">
          <v-progress-linear
            slot="activator"
            :value="item.componentValue"
            :buffer-value="bufferValue(item)"
            :color="color[hom]"
            height="45"
            :background-color="
              item.componentLongTermGoal ? 'grey' : 'grey lighten-2'
            "
            class="grey lighten-2 text-xs-left my-1"
            buffer
          >
            <v-layout justify-end>
              <h1 class="title font-weight-regular pa-3">
                {{ item.componentValue }}%
              </h1>
            </v-layout>
          </v-progress-linear>
        </div>
        <v-card color="transparent" dark>
          <v-card-text class="title font-weight-regular pb-0">
            Homeless
          </v-card-text>
          <v-card-text class="pt-0">
            <v-list class="transparent">
              <v-list-tile v-for="(item, ls) in homlsFacts" :key="ls">
                <v-list-tile-avatar>
                  <v-chip :color="color[ls]" class="black--text">
                    {{ item.componentValue }}%
                  </v-chip>
                </v-list-tile-avatar>
                <v-list-tile-title>{{
                  item.entityDisplayName
                }}</v-list-tile-title>
              </v-list-tile>
            </v-list>
          </v-card-text>
        </v-card>
      </v-tooltip>
    </div>

    <div
      v-if="
        (entity.entityType === 'District' && specedFacts.length === 2) ||
          (entity.entityType === 'School' && specedFacts.length === 3)
      "
      class="my-3"
    >
      <h1 class="title font-weight-regular text-xs-left">
        Students with Disabilities
      </h1>
      <v-tooltip top color="grey darken-4">
        <div v-for="(item, spe) in specedFacts" :key="spe" slot="activator">
          <v-progress-linear
            slot="activator"
            :value="item.componentValue"
            :buffer-value="bufferValue(item)"
            :color="color[spe]"
            height="45"
            :background-color="
              item.componentLongTermGoal ? 'grey' : 'grey lighten-2'
            "
            class="grey lighten-2 text-xs-left my-1"
            buffer
          >
            <v-layout justify-end>
              <h1 class="title font-weight-regular pa-3">
                {{ item.componentValue }}%
              </h1>
            </v-layout>
          </v-progress-linear>
        </div>
        <v-card color="transparent" dark>
          <v-card-text class="title font-weight-regular pb-0">
            Students with Disabilities
          </v-card-text>
          <v-card-text class="pt-0">
            <v-list class="transparent">
              <v-list-tile v-for="(item, spec) in specedFacts" :key="spec">
                <v-list-tile-avatar>
                  <v-chip :color="color[spec]" class="black--text">
                    {{ item.componentValue }}%
                  </v-chip>
                </v-list-tile-avatar>
                <v-list-tile-title>{{
                  item.entityDisplayName
                }}</v-list-tile-title>
              </v-list-tile>
            </v-list>
          </v-card-text>
        </v-card>
      </v-tooltip>
    </div>

    <div
      v-if="
        (entity.entityType === 'District' && nspecedFacts.length === 2) ||
          (entity.entityType === 'School' && nspecedFacts.length === 3)
      "
      class="my-3"
    >
      <h1 class="title font-weight-regular text-xs-left">
        Students without Disabilities
      </h1>
      <v-tooltip top color="grey darken-4">
        <div
          v-for="(item, nspec) in nspecedFacts"
          :key="nspec"
          slot="activator"
        >
          <v-progress-linear
            slot="activator"
            :value="item.componentValue"
            :buffer-value="bufferValue(item)"
            :color="color[nspec]"
            height="45"
            :background-color="
              item.componentLongTermGoal ? 'grey' : 'grey lighten-2'
            "
            class="grey lighten-2 text-xs-left my-1"
            buffer
          >
            <v-layout justify-end>
              <h1 class="title font-weight-regular pa-3">
                {{ item.componentValue }}%
              </h1>
            </v-layout>
          </v-progress-linear>
        </div>
        <v-card color="transparent" dark>
          <v-card-text class="title font-weight-regular pb-0">
            Students without Disabilities
          </v-card-text>
          <v-card-text class="pt-0">
            <v-list class="transparent">
              <v-list-tile
                v-for="(item, nspeced) in nspecedFacts"
                :key="nspeced"
              >
                <v-list-tile-avatar>
                  <v-chip :color="color[nspeced]" class="black--text">
                    {{ item.componentValue }}%
                  </v-chip>
                </v-list-tile-avatar>
                <v-list-tile-title>{{
                  item.entityDisplayName
                }}</v-list-tile-title>
              </v-list-tile>
            </v-list>
          </v-card-text>
        </v-card>
      </v-tooltip>
    </div>

    <div
      v-if="
        (entity.entityType === 'District' && englnrFacts.length === 2) ||
          (entity.entityType === 'School' && englnrFacts.length === 3)
      "
      class="my-3"
    >
      <h1 class="title font-weight-regular text-xs-left">
        English Learner
      </h1>
      <v-tooltip top color="grey darken-4">
        <div v-for="(item, eng) in englnrFacts" :key="eng" slot="activator">
          <v-progress-linear
            slot="activator"
            :value="item.componentValue"
            :buffer-value="bufferValue(item)"
            :color="color[eng]"
            height="45"
            :background-color="
              item.componentLongTermGoal ? 'grey' : 'grey lighten-2'
            "
            class="grey lighten-2 text-xs-left my-1"
            buffer
          >
            <v-layout justify-end>
              <h1 class="title font-weight-regular pa-3">
                {{ item.componentValue }}%
              </h1>
            </v-layout>
          </v-progress-linear>
        </div>
        <v-card color="transparent" dark>
          <v-card-text class="title font-weight-regular pb-0">
            English Learner
          </v-card-text>
          <v-card-text class="pt-0">
            <v-list class="transparent">
              <v-list-tile
                v-for="(item, lnr) in englnrFacts"
                :key="item.componentName"
              >
                <v-list-tile-avatar>
                  <v-chip :color="color[lnr]" class="black--text">
                    {{ item.componentValue }}%
                  </v-chip>
                </v-list-tile-avatar>
                <v-list-tile-title>{{
                  item.entityDisplayName
                }}</v-list-tile-title>
              </v-list-tile>
            </v-list>
          </v-card-text>
        </v-card>
      </v-tooltip>
    </div>

    <div
      v-if="
        (entity.entityType === 'District' && mltcnFacts.length === 2) ||
          (entity.entityType === 'School' && mltcnFacts.length === 3)
      "
      class="my-3"
    >
      <h1 class="title font-weight-regular text-xs-left">
        Military Connected
      </h1>
      <v-tooltip top color="grey darken-4">
        <div v-for="(item, mil) in mltcnFacts" :key="mil" slot="activator">
          <v-progress-linear
            slot="activator"
            :value="item.componentValue"
            :buffer-value="bufferValue(item)"
            :color="color[mil]"
            height="45"
            :background-color="
              item.componentLongTermGoal ? 'grey' : 'grey lighten-2'
            "
            class="grey lighten-2 text-xs-left my-1"
            buffer
          >
            <v-layout justify-end>
              <h1 class="title font-weight-regular pa-3">
                {{ item.componentValue }}%
              </h1>
            </v-layout>
          </v-progress-linear>
        </div>
        <v-card color="transparent" dark>
          <v-card-text class="title font-weight-regular pb-0">
            Military Connected
          </v-card-text>
          <v-card-text class="pt-0">
            <v-list class="transparent">
              <v-list-tile v-for="(item, mlt) in mltcnFacts" :key="mlt">
                <v-list-tile-avatar>
                  <v-chip :color="color[mlt]" class="black--text">
                    {{ item.componentValue }}%
                  </v-chip>
                </v-list-tile-avatar>
                <v-list-tile-title>{{
                  item.entityDisplayName
                }}</v-list-tile-title>
              </v-list-tile>
            </v-list>
          </v-card-text>
        </v-card>
      </v-tooltip>
    </div>
    <br />
  </div>
</template>

<script>
// import { mapGetters } from 'vuex'
import facts from '@/assets/data/facts.json'
import entities from '@/assets/data/entities.json'

export default {
  data() {
    return {
      color: ['amber', 'teal accent-3', 'light-blue accent-2'],
      entityType: ['State', 'District', 'School'],
      facts,
      entities
    }
  },
  computed: {
    // ...mapGetters({ entity: 'getSingleEntity' }),
    // ...mapGetters({ allFacts: 'getDetailsFacts' }),
    // ...mapGetters({ newFacts: 'getNewDetailsFacts' }),
    allFacts() {
      return this.facts.filter(
        fact =>
          (fact.entityId === this.$route.params.id ||
            fact.entityId === '0000-000' ||
            fact.entityId === fact.parentEntityId) &&
          fact.componentCode === this.$route.params.component.toUpperCase()
      )
    },
    entity() {
      const entity = this.entities.filter(
        entity => entity.entityId === this.$route.params.id
      )

      return entity[0]
    },
    femaleFacts() {
      return this.allFacts.filter(
        item =>
          item.aggregationLevelCategoryCode === 'GEN' &&
          item.aggregationCode === 'F'
      )
      // .reverse()
    },
    maleFacts() {
      return this.allFacts.filter(
        item =>
          item.aggregationLevelCategoryCode === 'GEN' &&
          item.aggregationCode === 'M'
      )
      // .reverse()
    },
    wFacts() {
      return this.allFacts
        .filter(
          item =>
            item.aggregationLevelCategoryCode === 'ETH' &&
            item.aggregationCode === 'W'
        )
        .reverse()
    },
    naFacts() {
      return this.allFacts
        .filter(
          item =>
            item.aggregationLevelCategoryCode === 'ETH' &&
            item.aggregationCode === 'NA'
        )
        .reverse()
    },
    asFacts() {
      return this.allFacts
        .filter(
          item =>
            item.aggregationLevelCategoryCode === 'ETH' &&
            item.aggregationCode === 'AS'
        )
        .reverse()
    },
    blFacts() {
      return this.allFacts
        .filter(
          item =>
            item.aggregationLevelCategoryCode === 'ETH' &&
            item.aggregationCode === 'B'
        )
        .reverse()
    },
    hFacts() {
      return this.allFacts
        .filter(
          item =>
            item.aggregationLevelCategoryCode === 'ETH' &&
            item.aggregationCode === 'H'
        )
        .reverse()
    },
    tmFacts() {
      return this.allFacts
        .filter(
          item =>
            item.aggregationLevelCategoryCode === 'ETH' &&
            item.aggregationCode === 'TM'
        )
        .reverse()
    },
    edisFacts() {
      return this.allFacts
        .filter(
          item =>
            item.aggregationLevelCategoryCode === 'OTH' &&
            item.aggregationCode === 'EDIS'
        )
        .reverse()
    },
    nedisFacts() {
      return this.allFacts
        .filter(
          item =>
            item.aggregationLevelCategoryCode === 'OTH' &&
            item.aggregationCode === 'NEDIS'
        )
        .reverse()
    },
    homlsFacts() {
      return this.allFacts
        .filter(
          item =>
            item.aggregationLevelCategoryCode === 'OTH' &&
            item.aggregationCode === 'HOMLS'
        )
        .reverse()
    },
    specedFacts() {
      return this.allFacts
        .filter(
          item =>
            item.aggregationLevelCategoryCode === 'OTH' &&
            item.aggregationCode === 'SPECED'
        )
        .reverse()
    },
    nspecedFacts() {
      return this.allFacts
        .filter(
          item =>
            item.aggregationLevelCategoryCode === 'OTH' &&
            item.aggregationCode === 'NSPECED'
        )
        .reverse()
    },
    englnrFacts() {
      return this.allFacts.filter(
        item =>
          item.aggregationLevelCategoryCode === 'OTH' &&
          item.aggregationCode === 'ENGLNR'
      )
    },
    mltcnFacts() {
      return this.allFacts.filter(
        item =>
          item.aggregationLevelCategoryCode === 'OTH' &&
          item.aggregationCode === 'MLTCN'
      )
    },
    otherFacts() {
      return this.allFacts.filter(
        item => item.aggregationLevelCategoryCode === 'OTH'
      )
    }
  },
  methods: {
    bufferValue(item) {
      if (item.componentLongTermGoal) return item.componentLongTermGoal
    }
  }
}
</script>

<style></style>
