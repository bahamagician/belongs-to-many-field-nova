<template>
  <default-field :field="field" :errors="errors">
    <template slot="field">
      <multi-select
        :options="options"
        :multiple="true"
        :label="column"
        track-by="name"
        :class="errorClasses"
        :placeholder="field.name"
        v-model="value"
      />
    </template>
  </default-field>
</template>

<script>
import { FormField, HandlesValidationErrors } from "laravel-nova";
import MultiSelect from "vue-multiselect";

export default {
  mixins: [FormField, HandlesValidationErrors],

  props: ["resourceName", "resourceId", "field"],

  components: {
    MultiSelect
  },

  data() {
    return {
      options: [],
      column: ""
    };
  },

  methods: {
    /*
     * Set the initial, internal value for the field.
     */
    setInitialValue() {
      this.options = this.field.options;
      this.column = this.field.column;
      this.value = this.field.value || "";
    },

    /**
     * Fill the given FormData object with the field's internal value.
     */
    fill(formData) {
      formData.append(this.field.attribute, JSON.stringify(this.value) || "");
    },

    /**
     * Update the field's internal value.
     */
    handleChange(value) {
      this.value = value;
    }
  }
};
</script>

<style src="vue-multiselect/dist/vue-multiselect.min.css"></style>