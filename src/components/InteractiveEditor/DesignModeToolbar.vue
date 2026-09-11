<template>
  <div class="design-mode-toolbar" v-if="isActive">
    <div class="toolbar-left">
      <button class="toolbar-btn" @click="toggleDesignMode">
        <svg width="16" height="16" viewBox="0 0 24 24">
          <path d="M12 20h9"/><path d="M16.5 3.5a2.121 2.121 0 0 1 3 3L7 19l-4 1 1-4L16.5 3.5z"/>
        </svg>
        Design
      </button>
      <button class="toolbar-btn" @click="selectElement">
        <svg width="16" height="16" viewBox="0 0 24 24">
          <path d="M3 3l7.07 16.97 2.51-7.39 7.39-2.51L3 3z"/>
        </svg>
        Select
      </button>
    </div>
    <div class="toolbar-center">
      <div class="ai-input-wrapper">
        <span class="ai-icon">✦</span>
        <input
          type="text"
          class="ai-input"
          placeholder="Describe a design change... e.g. 'Add a gradient'"
          disabled
        />
        <span class="ai-hint">Ctrl+I</span>
      </div>
    </div>
    <div class="toolbar-right">
      <button class="toolbar-btn" @click="undo">
        <svg width="14" height="14" viewBox="0 0 24 24">
          <polyline points="1 4 1 10 7 10"/><path d="M3.51 15a9 9 0 1 0 2.13-9.36L1 10"/>
        </svg>
        Undo
      </button>
      <button class="toolbar-btn" @click="redo">
        <svg width="14" height="14" viewBox="0 0 24 24">
          <polyline points="23 4 23 10 17 10"/><path d="M20.49 15a9 9 0 1 1-2.13-9.36L23 10"/>
        </svg>
        Redo
      </button>
      <button class="toolbar-btn" @click="resetChanges">Reset</button>
      <button class="toolbar-btn toolbar-btn-primary" @click="commitChanges">
        Commit
      </button>
      <span class="changes-badge">Changes ready</span>
    </div>
  </div>
</template>

<script>
import { mapState } from 'vuex';

export default {
  name: 'DesignModeToolbar',
  computed: {
    ...mapState(['editMode']),
  },
  methods: {
    toggleDesignMode() {
      this.$store.commit('SET_EDIT_MODE', !this.editMode);
    },
    selectElement() {},
    undo() {},
    redo() {},
    resetChanges() {},
    commitChanges() {},
  },
};
</script>

<style scoped lang="scss">
@import '@/styles/media-queries.scss';

.design-mode-toolbar {
  position: fixed;
  bottom: 0;
  left: 0;
  right: 0;
  height: var(--design-mode-toolbar-height, 3rem);
  background: linear-gradient(0deg, var(--design-mode-toolbar-bg), var(--surface-2));
  border-top: 1px solid var(--border-subtle);
  box-shadow: 0 -4px 16px rgba(0, 0, 0, 0.3), 0 0 8px rgba(59, 130, 246, 0.08);
  z-index: 10;
  display: flex;
  align-items: center;
  padding: 0 0.75rem;
  gap: 0.5rem;
  backdrop-filter: blur(12px);

  .toolbar-left {
    display: flex;
    align-items: center;
    gap: 0.35rem;
    flex-shrink: 0;
  }
  .toolbar-center {
    flex: 1;
    display: flex;
    justify-content: center;
  }
  .toolbar-right {
    display: flex;
    align-items: center;
    gap: 0.35rem;
    flex-shrink: 0;
  }

  .toolbar-btn {
    display: flex;
    align-items: center;
    gap: 0.3rem;
    padding: 0.25rem 0.5rem;
    font-size: 0.7rem;
    font-weight: 600;
    color: var(--text-secondary);
    background: var(--surface-2);
    border: 1px solid var(--border-subtle);
    border-radius: var(--curve-factor-small);
    cursor: pointer;
    transition: all 0.15s;
    white-space: nowrap;
    &:hover { color: var(--primary); border-color: var(--primary); }
    &:disabled { opacity: 0.4; cursor: not-allowed; }
  }
  .toolbar-btn-primary {
    color: #fff;
    background: linear-gradient(135deg, var(--primary), rgba(59, 130, 246, 0.8));
    border-color: var(--primary);
    &:hover { background: linear-gradient(135deg, #2563eb, var(--primary)); }
  }

  .ai-input-wrapper {
    display: flex;
    align-items: center;
    width: 100%;
    max-width: 400px;
    background: var(--surface-1);
    border: 1px solid var(--border-subtle);
    border-radius: var(--curve-factor);
    padding: 0.2rem 0.5rem;
    gap: 0.3rem;

    .ai-icon {
      color: var(--primary);
      font-size: 0.8rem;
      flex-shrink: 0;
    }
    .ai-input {
      flex: 1;
      border: none;
      background: transparent;
      font-size: 0.75rem;
      color: var(--text-primary);
      outline: none;
      &::placeholder { color: var(--text-muted); }
    }
    .ai-hint {
      font-size: 0.6rem;
      color: var(--text-muted);
      font-family: var(--font-monospace);
      background: var(--surface-2);
      padding: 0.1rem 0.3rem;
      border-radius: 2px;
      border: 1px solid var(--border-subtle);
      flex-shrink: 0;
    }
  }

  .changes-badge {
    font-size: 0.65rem;
    font-weight: 700;
    color: var(--success);
    background: rgba(34, 197, 94, 0.1);
    border: 1px solid rgba(34, 197, 94, 0.25);
    padding: 0.15rem 0.4rem;
    border-radius: 999px;
    white-space: nowrap;
  }

  @include phone {
    height: 2.5rem;
    .toolbar-btn span:not(.svg) { display: none; }
    .ai-input { max-width: 200px; }
    .changes-badge { display: none; }
  }
}
</style>
