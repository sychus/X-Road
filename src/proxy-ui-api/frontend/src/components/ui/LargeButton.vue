<!--
   The MIT License
   Copyright (c) 2019- Nordic Institute for Interoperability Solutions (NIIS)
   Copyright (c) 2018 Estonian Information System Authority (RIA),
   Nordic Institute for Interoperability Solutions (NIIS), Population Register Centre (VRK)
   Copyright (c) 2015-2017 Estonian Information System Authority (RIA), Population Register Centre (VRK)

   Permission is hereby granted, free of charge, to any person obtaining a copy
   of this software and associated documentation files (the "Software"), to deal
   in the Software without restriction, including without limitation the rights
   to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
   copies of the Software, and to permit persons to whom the Software is
   furnished to do so, subject to the following conditions:

   The above copyright notice and this permission notice shall be included in
   all copies or substantial portions of the Software.

   THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
   IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
   FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
   AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
   LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
   OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN
   THE SOFTWARE.
 -->
<template>
  <v-btn
    :outlined="outlined"
    :disabled="disabled"
    :min-width="min_width"
    :loading="loading"
    v-if="isAllowed"
    rounded
    color="primary"
    class="large-button"
    @click="click"
  >
    <slot></slot>
  </v-btn>
</template>

<script lang="ts">
/** Wrapper for vuetify button with x-road look */

import Vue, { PropType } from 'vue';
import { Permissions } from '@/global';

export default Vue.extend({
  props: {
    outlined: {
      type: Boolean,
      default: false,
    },
    // Set button disabled state
    disabled: {
      type: Boolean,
      default: false,
    },
    // Show loading spinner
    loading: {
      type: Boolean,
      default: false,
    },
    min_width: {
      type: Number,
      default: 120,
    },
    requiresPermission: {
      required: false,
      type: String as PropType<Permissions | undefined>,
      validator: (val) =>
        Object.values(Permissions).some((permission) => permission === val),
    },
  },
  computed: {
    isAllowed(): boolean {
      return (
        this.requiresPermission === undefined ||
        this.$store.getters.hasPermission(this.requiresPermission)
      );
    },
  },
  methods: {
    click(event: MouseEvent): void {
      this.$emit('click', event);
    },
  },
});
</script>

<style lang="scss" scoped>
$large-button-width: 140px;

.large-button {
  min-width: $large-button-width !important;
  border-radius: 4px;
  text-transform: uppercase;
  background-color: white;
}
</style>
