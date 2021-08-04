<template>
  <v-tooltip bottom nudge-right="50" :color="buttonStyle ? 'info' : 'secondary'">
    <template #activator="{ on, attrs }">
      <v-btn
        v-if="isFavorite || showAlways"
        small
        :color="buttonStyle ? 'info' : 'secondary'"
        :icon="!buttonStyle"
        :fab="buttonStyle"
        v-bind="attrs"
        @click.prevent="toggleFavorite"
        v-on="on"
      >
        <v-icon :small="!buttonStyle" :color="buttonStyle ? 'white' : 'secondary'">
          {{ isFavorite ? $globals.icons.heart : $globals.icons.heartOutline }}
        </v-icon>
      </v-btn>
    </template>
    <span>{{ isFavorite ? $t("recipe.remove-from-favorites") : $t("recipe.add-to-favorites") }}</span>
  </v-tooltip>
</template>

<script>
import { api } from "@/api";
import { defineComponent } from "@nuxtjs/composition-api";
import { useApiSingleton } from "~/composables/use-api";
export default defineComponent({
  props: {
    slug: {
      type: String,
      default: "",
    },
    showAlways: {
      type: Boolean,
      default: false,
    },
    buttonStyle: {
      type: Boolean,
      default: false,
    },
  },
  setup() {
    const api = useApiSingleton();

    return { api };
  },
  computed: {
    user() {
      return this.$auth.user;
    },
    isFavorite() {
      return this.$auth.user.favoriteRecipes.includes(this.slug);
    },
  },
  methods: {
    async toggleFavorite() {
      if (!this.isFavorite) {
        await this.api.users.addFavorite(this.$auth.user.id, this.slug);
      } else {
        await this.api.users.removeFavorite(this.$auth.user.id, this.slug);
      }
      this.$auth.fetchUser();
    },
  },
});
</script>

<style lang="scss" scoped>
</style>