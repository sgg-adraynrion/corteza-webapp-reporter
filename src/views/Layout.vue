<template>
  <div class="d-flex flex-column w-100 vh-100">
    <header>
      <c-topbar
        :sidebar-pinned="pinned"
      >
        <template #title>
          <portal-target
            name="topbar-title"
          />
        </template>

        <template #tools>
          <portal-target
            name="topbar-tools"
          />
        </template>
      </c-topbar>
    </header>

    <aside>
      <c-sidebar
        :expanded.sync="expanded"
        :pinned.sync="pinned"
        :icon="icon"
        :disabled-routes="['report.list']"
        expand-on-hover
      >
        <template #header-expanded>
          <portal-target name="sidebar-header-expanded" />
        </template>

        <template #body-expanded>
          <portal-target name="sidebar-body-expanded" />
        </template>

        <template #footer-expanded>
          <portal-target name="sidebar-footer-expanded" />
        </template>
      </c-sidebar>
    </aside>

    <main class="d-inline-flex h-100 overflow-auto">
      <!--
        Content spacer
        Large and xl screens should push in content when the nav is expanded
      -->
      <template>
        <div
          class="spacer"
          :class="{
            'expanded': expanded && pinned,
          }"
        />
      </template>

      <div
        class="d-flex flex-column w-100"
      >
        <router-view
          class="flex-grow-1 overflow-auto"
        />

        <portal-target
          name="report-toolbar"
        />
      </div>
    </main>
    <!-- <c-prompts /> -->
    <c-permissions-modal />
    <report-sidebar />
  </div>
</template>

<script>
import icon from '../themes/corteza-base/img/icon.png'
import { components } from '@cortezaproject/corteza-vue'
import ReportSidebar from 'corteza-webapp-reporter/src/components/ReportSidebar'
const { CPermissionsModal, CTopbar, CSidebar } = components

export default {
  components: {
    CPermissionsModal,
    CTopbar,
    CSidebar,
    ReportSidebar,
  },

  data () {
    return {
      expanded: undefined,
      pinned: undefined,
    }
  },

  computed: {
    icon () {
      return this.$Settings.attachment('ui.iconLogo', icon)
    },
  },

  created () {
    this.$root.$on('alert', this.displayToast)
  },

  methods: {
    displayToast ({ title, message, variant, countdown }) {
      this.$bvToast.toast(message, {
        title,
        variant,
        solid: true,
        autoHideDelay: countdown,
        toaster: 'b-toaster-bottom-right',
      })
    },
  },
}
</script>

<style lang="scss" scoped>
.spacer {
  min-width: 0;
  -webkit-transition: min-width 0.2s ease-in-out;
  -moz-transition: min-width 0.2s ease-in-out;
  -o-transition: min-width 0.2s ease-in-out;
  transition: min-width 0.2s ease-in-out;

  &.expanded {
    min-width: $sidebar-width;
    -webkit-transition: min-width 0.2s ease-in-out;
    -moz-transition: min-width 0.2s ease-in-out;
    -o-transition: min-width 0.2s ease-in-out;
    transition: min-width 0.2s ease-in-out;
  }
}
</style>
