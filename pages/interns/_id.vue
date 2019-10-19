<template>
  <v-card class="mx-auto" max-width="1280">
    <v-list-item>
      <v-img max-height="100" max-width="100" :src="intern_sheet[0].picture.url"></v-img>
      <v-list-item-content>
        <v-card-title v-html="intern_sheet[0].title"></v-card-title>
        <v-card-subtitle v-html="intern_sheet[0].subtitle"></v-card-subtitle>
      </v-list-item-content>
    </v-list-item>
    <v-card-text v-html="intern_sheet[0].content"></v-card-text>
  </v-card>
</template>

<script>
import Prismic from "prismic-javascript";
import PrismicDom from "prismic-dom";
import PrismicConfig from "~/prismic.config.js";

export default {
  async asyncData({ route, params }) {
    const api = await Prismic.getApi(PrismicConfig.apiEndpoint);
    let unformated_intern_sheet = [];
    const results = await api.query(
      Prismic.Predicates.at("my.blogpost.uid", route.params.id)
    );
    unformated_intern_sheet = results.results;

    const intern_sheet = unformated_intern_sheet.map(sheet => ({
      ...sheet,
      uid: sheet.uid,
      title: PrismicDom.RichText.asHtml(sheet.data.blog_post_title),
      subtitle: PrismicDom.RichText.asHtml(sheet.data.blog_work_departement),
      picture: sheet.data.blog_picture,
      content: PrismicDom.RichText.asHtml(sheet.data.blog_content)
    }));

    return {
      intern_sheet
    };
  }
};
</script>

<style scoped>
</style>