<template>
  <div class="themeSetting">
    <v-toolbar color="primary">
      <v-toolbar-title class="white--text">Theme Settings</v-toolbar-title>
    </v-toolbar>
    <v-container>
      <v-row column>
        <v-col>
          <v-select
            placeholder="Language"
            label="Language"
            :items="availableLanguages"
            v-model="$vuetify.lang.current"
            @change="changeLocale"
          />
          <v-subheader class="px-1 my-2">Color Option</v-subheader>
          <div class="color-option">
            <v-layout wrap>
              <label
                class="color-option--label flex xs6 pa-1"
                v-for="(option, index) in themeColorOptions"
                :key="index"
              >
                <input
                  type="radio"
                  name="color"
                  :value="option.key"
                  v-model="themeColor"
                  @click="handleChangeColor(option)"
                />
                <span class="color-option--item bg">
                  <span class="overlay" :class="option.key === theme ? 'selected' : ''">
                    <v-icon color="white">mdi-check</v-icon>
                  </span>
                  <span
                    class="color-option--item--header sideNav"
                    :class="option.value.sideNav"
                  ></span>
                  <span
                    class="color-option--item--header mainNav"
                    :class="option.value.mainNav"
                  ></span>
                  <span class="sideMenu" :class="option.value.sideMenu"></span>
                </span>
              </label>
            </v-layout>
          </div>
          <div class="theme-options">
            <v-subheader class="px-1 my-2">Sidebar Option</v-subheader>
            <v-divider></v-divider>
            <div class="my-3">
              <v-btn-toggle v-model="sideBarOption">
                <v-btn text value="dark">Dark</v-btn>
                <v-btn text value="light">Light</v-btn>
              </v-btn-toggle>
            </div>
          </div>
        </v-col>
      </v-row>
    </v-container>
  </div>
</template>
<script>
import colors from 'vuetify/es5/util/colors'
export default {
  data() {
    return {
      theme: this.$store.getters.getTheme,
      themeColor: this.$store.getters.getThemeColor,
      sideBarOption: 'light',
      colors: colors
    }
  },
  computed: {
    themeColorOptions() {
      return [
        {
          key: 'blue',
          value: {
            sideNav: 'blue',
            mainNav: 'blue',
            sideMenu: 'white'
          }
        },
        {
          key: 'teal',
          value: {
            sideNav: 'teal',
            mainNav: 'teal',
            sideMenu: 'white'
          }
        },
        {
          key: 'red',
          value: {
            sideNav: 'red',
            mainNav: 'red',
            sideMenu: 'white'
          }
        },
        {
          key: 'orange',
          value: {
            sideNav: 'orange',
            mainNav: 'orange',
            sideMenu: 'white'
          }
        },
        {
          key: 'purple',
          value: {
            sideNav: 'purple',
            mainNav: 'purple',
            sideMenu: 'white'
          }
        },
        {
          key: 'indigo',
          value: {
            sideNav: 'indigo',
            mainNav: 'indigo',
            sideMenu: 'white'
          }
        },
        {
          key: 'cyan',
          value: {
            sideNav: 'cyan',
            mainNav: 'cyan',
            sideMenu: 'white'
          }
        },
        {
          key: 'pink',
          value: {
            sideNav: 'pink',
            mainNav: 'pink',
            sideMenu: 'white'
          }
        },
        {
          key: 'green',
          value: {
            sideNav: 'green',
            mainNav: 'green',
            sideMenu: 'white'
          }
        }
      ]
    },
    availableLanguages() {
      const { locales } = this.$vuetify.lang
      return Object.keys(locales).map((lang) => {
        return {
          text: locales[lang].label,
          value: lang
        }
      })
    }
  },
  watch: {
    sideBarOption: {
      handler(val) {
        this.$vuetify.theme.dark = val === 'dark'
      },
      immediate: true
    },
    updateTheme() {}
  },
  methods: {
    changeLocale(lang) {
      this.$vuetify.lang.current = lang
    },
    handleChangeColor(option) {
      const color = this.colors[option.key].base
      this.$store.commit('setTheme', option.key)
      this.$store.commit('setThemeColor', color)
      this.$vuetify.theme.themes.light.primary = color
    }
  }
}
</script>
<style lang="sass" scoped>
.color-option
  &--label
    position: relative
    display: block
    cursor: pointer
    & input[type='radio']
      display: none
      &+span
        position: relative
        &>.overlay
          display: none
          position: absolute
          top: 0
          bottom: 0
          right: 0
          left: 0
          width: 100%
          height: 100%
          background-color: rgba(0, 0, 0, 0.3)
          text-align: center
          line-height: 30px
          color: #fff
      &:checked+span>.overlay
        display: block
    & .bg
      background-color: #f1f1f1
  &--item
    overflow: hidden
    display: block
    box-shadow: 0 0 2px rgba(0, 0, 0, 0.1)
    margin-bottom: 15px
    &--header
      height: 10px
    &>span
      display: block
      float: left
      width: 50%
      height: 20px
</style>
