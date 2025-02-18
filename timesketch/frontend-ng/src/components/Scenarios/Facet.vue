<!--
Copyright 2022 Google Inc. All rights reserved.

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

    http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.
-->
<template>
  <div>
    <v-row style="cursor: pointer" @click="toggleFacet()" :class="$vuetify.theme.dark ? 'dark-hover' : 'light-hover'">
      <v-col cols="1">
        <v-icon class="ml-2" v-if="!expanded">mdi-chevron-right</v-icon>
        <v-icon class="ml-2" v-else>mdi-chevron-down</v-icon>
      </v-col>
      <v-col cols="10">
        <span style="font-size: 0.9em">{{ facet.display_name }}</span>
      </v-col>
    </v-row>

    <v-expand-transition>
      <div v-show="expanded">
        <v-divider class="mt-3"></v-divider>
        <span
          @click="setActiveContext(question)"
          style="font-size: 0.9em"
          v-for="question in facet.questions"
          :key="question.id"
        >
          <ts-question :question="question"></ts-question>
        </span>
      </div>
    </v-expand-transition>
    <v-divider class="mt-3"></v-divider>
  </div>
</template>

<script>
import TsQuestion from './Question'

export default {
  props: ['scenario', 'facet'],
  components: { TsQuestion },
  data: function () {
    return {
      expanded: false,
    }
  },
  computed: {
    sketch() {
      return this.$store.state.sketch
    },
  },
  methods: {
    toggleFacet: function () {
      if (!this.expanded) {
        this.setActiveContext()
      } else {
        if (this.$store.state.activeContext.facet != null) {
          if (this.facet.id === this.$store.state.activeContext.facet.id) {
            this.$store.dispatch('clearActiveContext')
          }
        }
      }
      this.expanded = !this.expanded
    },
    setActiveContext: function (question) {
      let payload = {
        scenario: this.scenario,
        facet: this.facet,
        question: question,
      }
      this.$store.dispatch('setActiveContext', payload)
    },
  },
  created() {},
}
</script>
