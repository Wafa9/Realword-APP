<template>
  <div>
    <div v-if="isLoading" class="article-preview">Loading articles...</div>

    <!-- ################ move to ArticleMeta.vue Component -->
    <!-- ####### Move to Component ArticleShow-->

    <div v-else>
      <ArticleMeta
        v-for="(article, index) in articles"
        :article="article"
        :key="article.title + index + 1"
      />
      <VPagination :pages="pages" :currentPage.sync="currentPage" />
    </div>
  </div>
</template>

<script>
import { mapGetters } from "vuex";
import ArticleMeta from "./ArticleShow";
import VPagination from "./VPagination";
import TagList from "./TagList";
import { FETCH_ARTICLES } from "../store/actions.type";

export default {
  name: "ArticleList",
  components: {
    ArticleMeta,
    VPagination,
    TagList
  },
  props: {
    type: {
      type: String,
      required: false,
      default: "all"
    },
    author: {
      type: String,
      required: false
    },
    tag: {
      type: String,
      required: false
    },
    favorited: {
      type: String,
      required: false
    },
    itemsPerPage: {
      type: Number,
      required: false,
      default: 10
    },
    actions: {
      type: Boolean,
      required: false,
      default: false
    }
  },
  data() {
    return {
      currentPage: 1
    };
  },
  computed: {
    listConfig() {
      const { type } = this;
      const filters = {
        offset: (this.currentPage - 1) * this.itemsPerPage,
        limit: this.itemsPerPage
      };
      if (this.author) {
        filters.author = this.author;
      }
      if (this.tag) {
        filters.tag = this.tag;
      }
      if (this.favorited) {
        filters.favorited = this.favorited;
      }
      return {
        type,
        filters
      };
    },
    //  #### solving the "Loading Articles ..." issue preventing articles from appearing

    pages() {
      if (this.isLoading || this.articlesCount <= this.itemsPerPage) {
        return [];
      }
      return [
        ...Array(Math.ceil(this.articlesCount / this.itemsPerPage)).keys()
      ].map(e => e + 1);
    },
    // editArticleLink() {
    //   return { name: "article-edit", params: { slug: this.article.slug } };
    // },
    ...mapGetters([
      "articlesCount",
      "isLoading",
      "articles",
      "currentUser",
      "profile",
      "isAuthenticated"
    ])
  },
  watch: {
    currentPage(newValue) {
      this.listConfig.filters.offset = (newValue - 1) * this.itemsPerPage;
      this.fetchArticles();
    },
    type() {
      this.resetPagination();
      this.fetchArticles();
    },
    author() {
      this.resetPagination();
      this.fetchArticles();
    },
    tag() {
      this.resetPagination();
      this.fetchArticles();
    },
    favorited() {
      this.resetPagination();
      this.fetchArticles();
    }
  },
  mounted() {
    this.fetchArticles();
  },

  methods: {
    fetchArticles() {
      this.$store.dispatch(FETCH_ARTICLES, this.listConfig);
    },
    resetPagination() {
      this.listConfig.offset = 0;
      this.currentPage = 1;
    }
    // ############## move to ArticleMeta.vue Component
  }
};
</script>
