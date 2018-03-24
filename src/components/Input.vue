<template>
  <input type="search"
         autocorrect="off"
         autocapitalize="off"
         autocomplete="off"
         spellcheck="false"
         :class="bem()"
         v-model="query"
  >
</template>

<script>
import algoliaComponent from '../component';

export default {
  mixins: [algoliaComponent],
  data() {
    return {
      blockClassName: 'ais-input',
    };
  },
  computed: {
    query: {
      get() {
        return this.searchStore.query;
      },
      set(value) {
        this.searchStore.stop();
        this.searchStore.query = value;
        this.$emit('query', value);
        this.searchStore.start();
      },
    },
  },
  watch: {
    // whenever input search value changes
    query() {
      // We here ensure we give the time to listeners to alter the store's state
      // without triggering in between ghost queries.
      this.$nextTick(() => {
        this.searchStore.refresh();
      });
    },
  },
};
</script>
