<template>
  <v-app dark>
    <v-navigation-drawer fixed app>
      <v-list>
        <v-list-item v-for="(item, i) in items" :key="i" :to="item.to" router exact>
          <v-list-item-content>
            <v-list-item-title v-text="item.title" />
          </v-list-item-content>
        </v-list-item>
      </v-list>
    </v-navigation-drawer>
    <v-content>
      <v-container>
        <nuxt />
      </v-container>
    </v-content>
    <v-footer app>
      <span>&copy; 2019</span>
    </v-footer>
  </v-app>
</template>

<script>
export default {
  data() {
    return {
      items: [
        {
          title: "Welcome",
          to: "/"
        },
        {
          title: "Main staff",
          to: "/staff"
        },
        {
          title: "Interns",
          to: "/interns"
        }
      ],
      title: "Vuetify.js"
    };
  }
};
</script>
