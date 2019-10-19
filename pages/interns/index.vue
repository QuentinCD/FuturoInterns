<template>
  <div style="display: flex;">
    <div style="width: 90%;">
      <div>
        <v-card
          v-for="(post, index) in blog_posts"
          :key="post.uid"
          :id="post.uid"
          class="mx-auto"
          :class="{'dark-gray': index%2, 'gray': !(index%2) }"
        >
          <v-row>
            <v-col>
              <v-img max-height="250" :src="post.picture.url"></v-img>
            </v-col>
            <v-col>
              <v-card-title v-html="post.title"></v-card-title>
              <v-card-subtitle v-html="post.subtitle"></v-card-subtitle>
            </v-col>
          </v-row>
          <v-row>
            <v-card-text class="ma-5" v-html="post.content"></v-card-text>
          </v-row>
        </v-card>
      </div>
    </div>
    <div class="sidebar">
      <div v-for="post in blog_posts" :key="post.id" style="display: flex; justify-content: center">
        <nuxt-link :to="`/interns#${post.uid}`">
          <img class="icon" :src="post.picture.url" />
        </nuxt-link>
      </div>
    </div>
  </div>
</template>

<script>
import Prismic from "prismic-javascript";
import PrismicDom from "prismic-dom";
import PrismicConfig from "~/prismic.config.js";

export default {
  async asyncData() {
    const api = await Prismic.getApi(PrismicConfig.apiEndpoint);
    let unformated_blog_posts = [];
    const results = await api.query(
      Prismic.Predicates.at("document.type", "blogpost")
    );
    unformated_blog_posts = results.results;

    const blog_posts = unformated_blog_posts.map(post => ({
      ...post,
      uid: post.uid,
      title: PrismicDom.RichText.asHtml(post.data.blog_post_title),
      subtitle: PrismicDom.RichText.asHtml(post.data.blog_work_departement),
      picture: post.data.blog_picture,
      content: PrismicDom.RichText.asHtml(post.data.blog_content)
    }));

    return {
      blog_posts
    };
  }
};
</script>

<style scoped>
.sidebar {
  position: fixed;
  right: 0;
  width: 10%;
}

.icon {
  border-radius: 50%;
  width: 100px;
  height: auto;
}
.gray {
  background-color: #424242;
}
.dark-gray {
  background-color: #252525;
}
</style>