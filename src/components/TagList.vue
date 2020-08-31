<template>
  <ul class="tag-list">
    <li
      class="tag-default tag-pill tag-outline"
      v-for="(tag, index) of filteredTags"
      :key="index"
    >
      <span v-text="tag" />
    </li>
  </ul>
</template>

<script>
export default {
  name: "TagList",
  computed: {
    filteredTags() {
      return this.tags.filter(tag => tag !== "test");
    }
  },
  props: {
    tags: Array
  }
};
</script>
