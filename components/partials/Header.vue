<template lang="pug">
#pyconjp-header
    template(v-if="$vuetify.breakpoint.mdAndUp")
        v-toolbar#header(flat).white
            v-toolbar-title.ml-4.pa-2
                img(src="@/assets/images/horizontal_logo.png" width="180px").mt-2
            v-toolbar-title.grey--text.text--lighten-4.display-1 |
            .menu(v-for="item in items" :key="item.id")
                template(v-if="item.submenus.length === 0")
                    v-btn(flat).text-xs-center.menu-btn
                        span.subheading.font-weight-bold.textColor--text {{ item.name }}
                template(v-else)
                    v-menu(:nudge-width="100")
                        template(v-slot:activator="{ on }")
                            .btn(v-on="on")
                                v-btn(flat).text-xs-center.menu-btn
                                    span.subheading.font-weight-bold.textColor--text {{ item.name }}
                                    v-icon.textColor--text arrow_drop_down
                        v-list
                            v-list-tile(v-for='submenu in item.submenus' :key="submenu.id")
                                v-list-tile-title(v-text="submenu.name")
            v-spacer
            nuxt-link(:to="toLocale")
                v-btn(outline round)
                    v-icon(small).tertiary--text fas fa-globe
                    span.ml-2.textColor--text {{ toLang }}
            v-toolbar-items.ml-2
                v-btn(small color="primary").depressed.subheading
                    span {{ $t('header.apply') }}
                    v-icon expand_more
    template(v-else="$vuetify.breakpoint.smAndDown")
        v-toolbar#header(fixed inverted-scroll).white.pa-2
            v-toolbar-title.ml-2
                img(src="@/assets/images/horizontal_logo.png" width="150px")
            v-spacer
            v-btn(flat icon)
                v-icon(medium).tertiary--text fas fa-globe
            v-btn(flat icon).mr-2
                v-icon(medium) menu
</template>

<style lang="sass">
#header .v-toolbar__content
    padding: 0 !important

.menu-btn
    padding: 0 !important
</style>


<script>
import { mapState, mapActions } from 'vuex';

export default {
    name: "pycon-header",
    data() {
        return {
        }
    },
    computed: {
        ...mapState({
            locale: state => state.locale
        }),
        toLang() {
            return this.locale === "ja" ? "English" : "日本語"
        },
        toLocale() {
            switch(this.locale) {
                case "ja":
                    return "/en" + this.$route.fullPath
                case "en":
                    return "/ja" + this.$route.fullPath.replace("en/", "")
            }
        },
        items() {
            return [
                { id: 1, name: this.$t("header.menu.home"), submenus: [] },
                { id: 2, name: this.$t("header.menu.about"), submenus: [{id: 1, name: "◯◯について"}, {id: 2, name: "△△について"}] },
                { id: 3, name: this.$t("header.menu.events"), submenus: [{id: 1, name: "Conference"}, {id: 2, name: "Tutorial Day"}] },
                { id: 4, name: this.$t("header.menu.news"), submenus: [] },
                { id: 5, name: this.$t("header.menu.access"), submenus: [] },
                { id: 6, name: this.$t("header.menu.support"), submenus: [] },
                { id: 7, name: this.$t("header.menu.sponsor"), submenus: [] }
            ]
        }
    },
    methods: {
        ...mapActions(["switchLang"])
    }
}
</script>

