<template>
  <ul>
    <li v-for="page in pages">
      <post v-bind:page="page" />
    </li>
  </ul>
</template>

<script lang="ts">
import Vue from "vue";
export default Vue.extend({
  props: ["dirNames", "limit"],
  computed: {
    pages() {
      const { pages } = this.$site;
      return pages
        .filter(
          page =>
            this.dirNames.some(dirname =>
              page.path.startsWith(`/${dirname}/`)
            ) && this.dirNames.every(dirName => page.path !== `/${dirName}/`)
        )
        .filter(page => !page.frontmatter.disallowIndex)
        .sort((x, y) => {
          return ((x.frontmatter.createdAt || '0') < (y.frontmatter.createdAt || '0') ? 1 : -1)
        })
        .slice(0, this.limit || Number.MAX_SAFE_INTEGER);
    }
  }
});
</script>

<style lang="stylus" scoped>
ul {
  list-style: none;
  padding: 0;
}
</style>