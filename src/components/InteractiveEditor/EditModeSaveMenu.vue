<template>
  <div class="edit-mode-bottom-banner">
    <div class="edit-banner-section intro-container" v-if="showEditMsg">
      <p class="section-sub-title edit-mode-intro l-1">
        {{ $t('interactive-editor.menu.edit-mode-subtitle') }}
      </p>
      <p class="edit-mode-intro l-2">
        {{ $t('interactive-editor.menu.edit-mode-description') }}
      </p>
    </div>
    <div class="edit-banner-section intro-container" v-else>
      <AccessError class="no-permission" />
    </div>
    <div class="edit-banner-section empty-space"></div>
    <div class="edit-banner-section save-buttons-container">
      <p class="section-sub-title">
        {{ $t('interactive-editor.menu.config-save-methods-subheading') }}
      </p>
      <Button
        :click="saveLocally"
        :disallow="!permissions.allowSaveLocally"
        v-tooltip="tooltip($t('interactive-editor.menu.save-locally-tooltip'))"
      >
        {{ $t('interactive-editor.menu.save-locally-btn') }}
        <SaveLocallyIcon />
      </Button>
      <Button
        :click="writeToDisk"
        :disallow="!permissions.allowWriteToDisk"
        v-tooltip="tooltip($t('interactive-editor.menu.save-disk-tooltip'))"
      >
        {{ $t('interactive-editor.menu.save-disk-btn') }}
        <SaveToDiskIcon />
      </Button>
      <Button
        :click="openExportConfigMenu"
        :disallow="!permissions.allowViewConfig"
        v-tooltip="tooltip($t('interactive-editor.menu.export-config-tooltip'))"
      >
        {{ $t('interactive-editor.menu.export-config-btn') }}
        <ExportIcon />
      </Button>
      <Button
        :click="reset"
        v-tooltip="tooltip($t('interactive-editor.menu.cancel-changes-tooltip'))"
      >
        {{ $t('interactive-editor.menu.cancel-changes-btn') }}
        <CancelIcon />
      </Button>
    </div>
    <div class="edit-banner-section edit-config-buttons-container">
      <p class="section-sub-title">
        {{ $t('interactive-editor.menu.edit-site-data-subheading') }}
      </p>
      <Button
        :click="openEditPageInfo"
        :disallow="!permissions.allowViewConfig"
        v-tooltip="tooltip($t('interactive-editor.menu.edit-page-info-tooltip'))"
      >
        {{ $t('interactive-editor.menu.edit-page-info-btn') }}
        <PageInfoIcon />
      </Button>
      <Button
        :click="openEditAppConfig"
        :disallow="!permissions.allowViewConfig"
        v-tooltip="tooltip($t('interactive-editor.menu.edit-app-config-tooltip'))"
      >
        {{ $t('interactive-editor.menu.edit-app-config-btn') }}
        <AppConfigIcon />
      </Button>
      <Button
        :click="openEditMultiPages"
        :disallow="!permissions.allowViewConfig"
        v-tooltip="tooltip($t('interactive-editor.menu.edit-pages-tooltip'))"
      >
        {{ $t('interactive-editor.menu.edit-pages-btn') }}
        <MultiPagesIcon />
      </Button>
    </div>
    <EditPageInfo />
    <EditAppConfig />
    <EditMultiPages />
  </div>
</template>

<script>
import ConfigSavingMixin from '@/mixins/ConfigSaving';
import Button from '@/components/FormElements/Button';
import StoreKeys from '@/utils/StoreMutations';
import EditPageInfo from '@/components/InteractiveEditor/EditPageInfo';
import EditAppConfig from '@/components/InteractiveEditor/EditAppConfig';
import EditMultiPages from '@/components/InteractiveEditor/EditMultiPages';
import { modalNames } from '@/utils/defaults';
import AccessError from '@/components/Configuration/AccessError';
import SaveLocallyIcon from '@/assets/interface-icons/interactive-editor-save-locally.svg';
import SaveToDiskIcon from '@/assets/interface-icons/interactive-editor-save-disk.svg';
import ExportIcon from '@/assets/interface-icons/interactive-editor-export-changes.svg';
import CancelIcon from '@/assets/interface-icons/interactive-editor-cancel-changes.svg';
import AppConfigIcon from '@/assets/interface-icons/interactive-editor-app-config.svg';
import PageInfoIcon from '@/assets/interface-icons/interactive-editor-page-info.svg';
import MultiPagesIcon from '@/assets/interface-icons/config-pages.svg';

export default {
  name: 'EditModeSaveMenu',
  mixins: [ConfigSavingMixin],
  components: {
    Button,
    EditPageInfo,
    EditAppConfig,
    EditMultiPages,
    SaveLocallyIcon,
    SaveToDiskIcon,
    ExportIcon,
    CancelIcon,
    AppConfigIcon,
    PageInfoIcon,
    MultiPagesIcon,
    AccessError,
  },
  computed: {
    config() { return this.$store.state.config; },
    permissions() { return this.$store.getters.permissions; },
    showEditMsg() {
      return this.permissions.allowWriteToDisk
        || this.permissions.allowSaveLocally;
    },
  },
  methods: {
    reset() {
      this.$store.dispatch(StoreKeys.INITIALIZE_CONFIG);
      this.$store.commit(StoreKeys.SET_EDIT_MODE, false);
    },
    openExportConfigMenu() {
      this.$modal.show(modalNames.EXPORT_CONFIG_MENU);
      this.$store.commit(StoreKeys.SET_MODAL_OPEN, true);
    },
    openEditPageInfo() {
      this.$modal.show(modalNames.EDIT_PAGE_INFO);
      this.$store.commit(StoreKeys.SET_MODAL_OPEN, true);
    },
    openEditAppConfig() {
      this.$modal.show(modalNames.EDIT_APP_CONFIG);
      this.$store.commit(StoreKeys.SET_MODAL_OPEN, true);
    },
    openEditMultiPages() {
      this.$modal.show(modalNames.EDIT_MULTI_PAGES);
      this.$store.commit(StoreKeys.SET_MODAL_OPEN, true);
    },
    tooltip(content) {
      return { content, trigger: 'hover focus', delay: 250 };
    },
    showToast(message, success) {
      this.$toasted.show(message, {
        className: `toast-${success ? 'success' : 'error'}`,
      });
    },
    saveLocally() {
      const msg = this.$t('interactive-editor.menu.save-locally-warning');
      const confirmed = window.confirm(msg);
      if (confirmed) { this.saveConfigLocally(this.config); }
    },
    writeToDisk() { this.writeConfigToDisk(this.config); },
  },
};
</script>

<style scoped lang="scss">
@import '@/styles/media-queries.scss';

div.edit-mode-bottom-banner {
  position: fixed;
  display: grid;
  z-index: 5;
  bottom: 0;
  width: 100%;
  padding: 0.25rem 0;
  border-top: 1px solid var(--border-subtle);
  background: linear-gradient(180deg, var(--surface-2), var(--surface-1));
  box-shadow: 0 -4px 12px rgba(0, 0, 0, 0.4), 0 0 8px rgba(59, 130, 246, 0.1);
  grid-template-columns: 45% 10% 45%;
  backdrop-filter: blur(8px);
  @include laptop-up { grid-template-columns: 50% 10% 40%; }
  @include monitor-up { grid-template-columns: 40% 30% 30%; }
  @include big-screen-up { grid-template-columns: 25% 50% 25%; }

  .edit-banner-section {
    padding: 0.5rem;
    height: fit-content;
    display: grid;
    p.section-sub-title {
      margin: 0;
      color: var(--primary);
      font-weight: 700;
      font-size: 0.75rem;
      letter-spacing: 0.05em;
      text-transform: uppercase;
      cursor: default;
    }
    &.intro-container p.edit-mode-intro {
      margin: 0;
      color: var(--text-secondary);
      font-size: 0.8rem;
      cursor: default;
    }
    .no-permission {
      margin: 0;
      width: auto;
      padding: 0 0.5rem;
    }
  }

  button {
    margin: 0.15rem;
    height: stretch;
    max-height: 2.5rem;
    font-size: 0.75rem;
    padding: 0.25rem 0.5rem;
    border-radius: var(--curve-factor);
    border: 1px solid var(--border-subtle);
    background: var(--surface-2);
    color: var(--text-secondary);
    cursor: pointer;
    transition: all 0.15s;
    &:hover:not(.disallowed) {
      color: var(--primary);
      border-color: var(--primary);
      background: rgba(59, 130, 246, 0.1);
    }
    &.disallowed {
      opacity: 0.4;
      cursor: not-allowed;
    }
  }

  &.edit-config-buttons-container {
    grid-template-columns: repeat(3, 1fr);
    p.section-sub-title { grid-column-start: span 3; }
  }
  &.save-buttons-container {
    grid-row-start: span 1;
    grid-template-columns: repeat(2, 1fr);
    p.section-sub-title { grid-column-start: span 2; }
  }
}

@include tablet-down {
  div.edit-mode-bottom-banner {
    display: flex;
    flex-direction: column;
    .edit-banner-section {
      max-width: 90%;
      width: 100%;
      margin: 0.1rem auto;
    }
  }
}
</style>
