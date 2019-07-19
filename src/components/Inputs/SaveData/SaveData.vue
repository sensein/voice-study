<template>
  <div class="textInput">
    <b-form @submit="onSubmit">
      <b-btn type="submit">Upload</b-btn>
    </b-form>
  </div>
</template>

<style>
</style>

<script>
export default {
  name: 'SaveData',
  props: ['constraints', 'init', 'result', 'selected_language'],
  methods: {
    onSubmit(e) {
      e.preventDefault();
      this.$emit('valueChanged', this.input);
    },
  },
  data() {
    return {
      input: '',
    };
  },
  mounted() {
    if (this.init) {
      this.input = this.init;
    }
  },
  computed: {
    showResult() {
      if (this.result) {
        this.$emit('valueChanged', this.result);
        return 'Success';
      } return 'Fail';
    },
  },
};
</script>
