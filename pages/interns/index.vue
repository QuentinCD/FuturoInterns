<template>
  <div>
    <carousel :perPage="3" :scrollPerPage="false">
      <slide v-for="post in blog_posts" :key="post.uid">
        <v-card class="mx-auto" max-width="500" :to="`/interns/${post.uid}`">
          <v-img max-height="250" :src="post.picture.url"></v-img>
          <v-card-title>{{ post.title }}</v-card-title>
          <v-card-subtitle>{{ post.subtitle }}</v-card-subtitle>
        </v-card>
      </slide>
    </carousel>
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
      title: PrismicDom.RichText.asText(post.data.blog_post_title),
      subtitle: PrismicDom.RichText.asText(post.data.blog_work_departement),
      picture: post.data.blog_picture,
      content: PrismicDom.RichText.asText(post.data.blog_content)
    }));

    return {
      blog_posts
    };
  }
};
</script>

<style>
</style>