<!--

  Component:    NoDataAlert
  Description:  App preferences panel

  Props:
  └── value: Value variable for binding of the value

  Events:
  └── close: Emitted on 'okay' click

-->

<template>
  <alert-overlay :value="value" @input="update" icon="mdi-database-remove-outline" type="error">
    <template>
      <div>
        <div class="text-h5 font-weight-black">Sorry,&nbsp;<span
            class="font-weight-bold">your search produced no results.</span></div>
        <div class="text-body-1 mt-1 mb-6">This is due to the lack of data in the selected period of analysis.</div>

        <div class="text-uppercase text-body-4 font-weight-black">What to do?</div>
        <div>Try selecting a larger geographic area, or a time period that includes more sequences.
          Using the Dataset Explorer can help you in this regard.
        </div>
      </div>
    </template>
  </alert-overlay>
</template>

<script>
import AlertOverlay from "@/components/general/basic/AlertOverlay";

export default {
  name: "NoDataAlert",
  components: {AlertOverlay},

  props: {
    /** Value variable for binding of the value */
    value: {},
  },

  methods: {
    /** On alert overlay updates */
    update(newVal, oldVal) {
      this.$emit('input', newVal)
      if(!newVal && (oldVal===true || oldVal===undefined)){
        this.$emit('close')
      }

    }
  }
}
</script>

<style scoped>

</style>