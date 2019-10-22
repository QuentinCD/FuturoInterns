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
            <v-col class="selfie">
              <v-img max-height="250" :src="post.picture.url">
                <span class="to_bot_left" v-html="post.title"></span>
              </v-img>
            </v-col>
            <v-col>
              <v-card-text class="ma-5" v-html="post.tasks"></v-card-text>
            </v-col>
          </v-row>
          <v-row></v-row>
        </v-card>
      </div>
    </div>
    <div class="sidebar">
      <div v-for="post in blog_posts" :key="post.id" style="display: flex; justify-content: center">
        <nuxt-link :to="`#${post.uid}`">
          <img class="icon" :src="post.avatar.url" />
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
      Prismic.Predicates.at("document.type", "intern_card")
    );
    unformated_blog_posts = results.results;

    const blog_posts = unformated_blog_posts.map(post => ({
      ...post,
      uid: post.uid,
      uid: post.uid,
      title: PrismicDom.RichText.asHtml(post.data.name_and_surname),
      subtitle: PrismicDom.RichText.asHtml(post.data.work_departement),
      picture: post.data.intern_picture,
      avatar: post.data.intern_avatar,
      tasks: PrismicDom.RichText.asHtml(post.data.work_tasks)
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
.to_bot_left {
  color: white;
  font-weight: bold;
  position: absolute;
  bottom: 10px;
  left: 20px;
}
.selfie {
  max-width: 50%;
}
</style>