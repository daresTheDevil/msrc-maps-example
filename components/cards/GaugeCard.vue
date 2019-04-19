<template>
  <v-card hover flat color="transparent">
    <v-card-text class="pb-0">
      <svg width="100%" viewBox="0 0 42 21" class="donut">
        <circle
          class="donut-hole"
          cx="21"
          cy="21"
          r="15.91549430918954"
          fill="transparent"
        />
        <circle
          class="donut-ring"
          cx="21"
          cy="21"
          r="15.91549430918954"
          fill="transparent"
          stroke="transparent"
          stroke-width="3"
        />
        <circle
          class="donut-segment"
          cx="21"
          cy="21"
          r="15.91549430918954"
          fill="transparent"
          stroke="#ccc"
          stroke-dasharray="50 50"
          stroke-dashoffset="50"
          stroke-width="3"
        />
        <circle
          :stroke-dasharray="dasharray"
          :stroke="color"
          class="donut-segment"
          cx="21"
          cy="21"
          r="15.91549430918954"
          fill="transparent"
          stroke-dashoffset="50"
          stroke-width="5"
        />
        <text
          x="21"
          y="20"
          font-size="7px"
          font-weight="bold"
          text-anchor="middle"
          alignment-baseline="central"
        >
          {{
            data.componentValue === 0
              ? '&lt; 5'
              : data.componentValue.toFixed(1)
          }}%
        </text>
      </svg>
    </v-card-text>

    <v-card-text class="pt-0">
      <h1
        v-if="$vuetify.breakpoint.smAndUp"
        class="title font-weight-regular text-xs-center"
      >
        {{ data.componentName }}
      </h1>
      <h1
        v-if="$vuetify.breakpoint.smAndDown"
        class="caption font-weight-regular text-xs-center"
      >
        {{ data.componentName }}
      </h1>
    </v-card-text>
  </v-card>
</template>

<script>
export default {
  props: {
    data: {
      type: [Object, Array, String, Number],
      default: null
    },
    color: {
      type: String,
      default: null
    }
  },
  data() {
    return {
      value: '',
      text: ''
    }
  },
  computed: {
    dasharray() {
      const value = this.data.componentValue
      return value / 2 + ' ' + (100 - value) / 2
    }
  }
}
</script>
