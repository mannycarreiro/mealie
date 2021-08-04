<template>
  <v-app dark>
    <!-- <TheSnackbar /> -->

    <AppSidebar
      v-model="sidebar"
      absolute
      :top-link="topLinks"
      :secondary-links="$auth.user.admin ? adminLinks : null"
      :bottom-links="$auth.user.admin ? bottomLinks : null"
      :user="{ data: true }"
      @input="sidebar = !sidebar"
    />

    <AppHeader>
      <v-btn icon @click.stop="sidebar = !sidebar">
        <v-icon> {{ $globals.icons.menu }}</v-icon>
      </v-btn>
    </AppHeader>
    <v-main>
      <v-scroll-x-transition>
        <Nuxt />
      </v-scroll-x-transition>
    </v-main>
  </v-app>
</template>
  

<script lang="ts">
import { defineComponent } from "@nuxtjs/composition-api";
import AppHeader from "@/components/Layout/AppHeader.vue";
import AppSidebar from "@/components/Layout/AppSidebar.vue";

export default defineComponent({
  components: { AppHeader, AppSidebar },
  middleware: "auth",
  auth: true,
  setup() {
    return {};
  },
  data() {
    return {
      sidebar: null,
      topLinks: [
        {
          icon: this.$globals.icons.user,
          to: "/user/profile",
          title: this.$t("sidebar.profile"),
        },
      ],
      adminLinks: [
        {
          icon: this.$globals.icons.viewDashboard,
          to: "/admin/dashboard",
          title: this.$t("sidebar.dashboard"),
        },
        {
          icon: this.$globals.icons.cog,
          to: "/admin/site-settings",
          title: this.$t("sidebar.site-settings"),
        },
        {
          icon: this.$globals.icons.tools,
          to: "/admin/toolbox",
          title: this.$t("sidebar.toolbox"),
        },
        {
          icon: this.$globals.icons.group,
          to: "/admin/manage-users",
          title: this.$t("sidebar.manage-users"),
        },
        {
          icon: this.$globals.icons.import,
          to: "/admin/migrations",
          title: this.$t("sidebar.migrations"),
        },
      ],
      bottomLinks: [
        {
          icon: this.$globals.icons.heart,
          title: this.$t("about.support"),
          href: "https://github.com/sponsors/hay-kot",
        },
        {
          icon: this.$globals.icons.information,
          title: this.$t("about.about"),
          to: "/admin/about",
        },
      ],
    };
  },
});
</script>
      
      <style scoped>
</style>+