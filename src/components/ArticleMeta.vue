<template>
  <div class="article-meta">
    <router-link
      :to="{ name: 'profile', params: { username: article.author.username } }"
    >
      <img :src="article.author.image" />
    </router-link>
    <div class="info">
      <router-link
        :to="{ name: 'profile', params: { username: article.author.username } }"
        class="author"
        >{{ article.author.username }}</router-link
      >
      <span class="date">{{ article.createdAt | date }}</span>
    </div>
    <rwv-article-actions
      v-if="actions"
      :article="article"
      :canModify="isCurrentUser()"
    ></rwv-article-actions>
    <span v-if="isCurrentUser(article)">
      <!-- <router-link class="btn btn-sm btn-outline-secondary" :to="editArticleLink">
        <i class="ion-edit"></i>
        <span>&nbsp;Edit Article</span>
      </router-link>-->
      <span>&nbsp;&nbsp;</span>
      <button class="btn btn-outline-danger btn-sm" @click="deleteArticle">
        <i class="ion-trash-a"></i>
        <span>&nbsp;Delete Article</span>
      </button>
    </span>
    <button
      v-else
      class="btn btn-sm pull-xs-right"
      @click="toggleFavorite"
      :class="{
        'btn-primary': article.favorited,
        'btn-outline-primary': !article.favorited
      }"
    >
      <i class="ion-heart"></i>
      <span class="counter">{{ article.favoritesCount }}</span>
    </button>
  </div>
</template>

<script>
import { mapGetters } from "vuex";

import {
  FAVORITE_ADD,
  FAVORITE_REMOVE,
  ARTICLE_DELETE
  // ARTICLE_EDIT
} from "@/store/actions.type";

export default {
  name: "ArticleMeta",

  props: {
    article: {
      type: Object,
      required: true
    },
    actions: {
      type: Boolean,
      required: false,
      default: false
    }
  },
  computed: {
    ...mapGetters(["currentUser", "isAuthenticated"])
    // editArticleLink() {
    //   return { name: "article-edit", params: { slug: this.article.slug } };
    // }
  },
  methods: {
    isCurrentUser() {
      if (this.currentUser.username && this.article.author.username) {
        return this.currentUser.username === this.article.author.username;
      }
      return false;
    },
    toggleFavorite() {
      if (!this.isAuthenticated) {
        this.$router.push({ name: "login" });
        return;
      }
      const action = this.article.favorited ? FAVORITE_REMOVE : FAVORITE_ADD;
      this.$store.dispatch(action, this.article.slug);
    },
    async deleteArticle() {
      try {
        await this.$store.dispatch(ARTICLE_DELETE, this.article.slug);
        this.$router.push("/");
      } catch (err) {
        console.error(err);
      }
    }
    // editArticleLink() {
    //   return { name: "article-edit", params: { slug: this.article.slug } };
    // }
  }
};
</script>
