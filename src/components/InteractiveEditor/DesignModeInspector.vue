<template>
  <div class="design-mode-inspector" v-if="isActive">
    <div class="inspector-header">
      <span class="inspector-title">Design Mode</span>
      <span class="inspector-status">● Active</span>
      <button class="inspector-close" @click="$emit('close')">✕</button>
    </div>
    <div class="inspector-tabs">
      <button
        v-for="tab in tabs"
        :key="tab"
        :class="['inspector-tab', { active: activeTab === tab }]"
        @click="activeTab = tab"
      >{{ tab }}</button>
    </div>
    <div class="inspector-content">
      <template v-if="activeTab === 'Style'">
        <div class="inspector-group">
          <label class="inspector-label">Typography</label>
          <select class="inspector-select">
            <option>Inter</option>
            <option>Raleway</option>
            <option>Inconsolata</option>
          </select>
        </div>
        <div class="inspector-group">
          <label class="inspector-label">Font Size</label>
          <input type="range" min="10" max="24" value="14" class="inspector-range" />
        </div>
        <div class="inspector-group">
          <label class="inspector-label">Colors & Backgrounds</label>
          <div class="inspector-color-row">
            <span>Text</span>
            <input type="color" value="#e8f0fe" class="inspector-color" />
          </div>
          <div class="inspector-color-row">
            <span>Background</span>
            <input type="color" value="#060b1a" class="inspector-color" />
          </div>
          <div class="inspector-color-row">
            <span>Border</span>
            <input type="color" value="#1e3a5f" class="inspector-color" />
          </div>
        </div>
        <div class="inspector-group">
          <label class="inspector-label">Layout & Spacing</label>
          <div class="inspector-slider-row">
            <span>Margin</span>
            <input type="range" min="0" max="32" value="8" class="inspector-range" />
          </div>
          <div class="inspector-slider-row">
            <span>Padding</span>
            <input type="range" min="0" max="32" value="8" class="inspector-range" />
          </div>
          <div class="inspector-slider-row">
            <span>Width</span>
            <input type="range" min="200" max="800" value="400" class="inspector-range" />
          </div>
        </div>
        <div class="inspector-group">
          <label class="inspector-label">Appearance & Borders</label>
          <div class="inspector-slider-row">
            <span>Border Radius</span>
            <input type="range" min="0" max="24" value="8" class="inspector-range" />
          </div>
          <div class="inspector-slider-row">
            <span>Border Width</span>
            <input type="range" min="0" max="4" value="1" class="inspector-range" />
          </div>
          <div class="inspector-slider-row">
            <span>Opacity</span>
            <input type="range" min="0" max="100" value="100" class="inspector-range" />
          </div>
        </div>
      </template>
      <template v-if="activeTab === 'Layout'">
        <div class="inspector-group">
          <label class="inspector-label">Columns</label>
          <select class="inspector-select">
            <option>Auto</option>
            <option>1</option>
            <option>2</option>
            <option>3</option>
            <option>4</option>
          </select>
        </div>
        <div class="inspector-group">
          <label class="inspector-label">Direction</label>
          <select class="inspector-select">
            <option>Vertical</option>
            <option>Horizontal</option>
          </select>
        </div>
      </template>
      <template v-if="activeTab === 'Advanced'">
        <div class="inspector-group">
          <label class="inspector-label">Custom CSS</label>
          <textarea class="inspector-textarea" placeholder="/* Custom styles */"></textarea>
        </div>
      </template>
      <template v-if="activeTab === 'Code'">
        <div class="inspector-group">
          <label class="inspector-label">Element Info</label>
          <div class="inspector-code-block">
            <div>Tag: div.item</div>
            <div>ID: —</div>
            <div>Class: item-wrapper</div>
          </div>
        </div>
      </template>
    </div>
  </div>
</template>

<script>
export default {
  name: 'DesignModeInspector',
  props: { isActive: Boolean },
  data() {
    return {
      activeTab: 'Style',
      tabs: ['Style', 'Layout', 'Advanced', 'Code'],
    };
  },
};
</script>

<style scoped lang="scss">
@import '@/styles/media-queries.scss';

.design-mode-inspector {
  position: fixed;
  top: var(--topbar-height);
  right: 0;
  bottom: 0;
  width: 300px;
  background: var(--design-mode-inspector-bg);
  border-left: 1px solid var(--border-subtle);
  box-shadow: -4px 0 16px rgba(0, 0, 0, 0.3);
  z-index: 10;
  display: flex;
  flex-direction: column;
  backdrop-filter: blur(12px);

  .inspector-header {
    display: flex;
    align-items: center;
    gap: 0.5rem;
    padding: 0.6rem 0.75rem;
    border-bottom: 1px solid var(--border-subtle);
    background: var(--surface-2);
    flex-shrink: 0;

    .inspector-title {
      font-size: 0.8rem;
      font-weight: 700;
      color: var(--text-primary);
      letter-spacing: 0.03em;
    }
    .inspector-status {
      font-size: 0.7rem;
      color: var(--success);
      margin-left: auto;
    }
    .inspector-close {
      width: 1.5rem;
      height: 1.5rem;
      border: 1px solid var(--border-subtle);
      border-radius: var(--curve-factor-small);
      background: var(--surface-1);
      color: var(--text-muted);
      cursor: pointer;
      font-size: 0.7rem;
      display: flex;
      align-items: center;
      justify-content: center;
      transition: all 0.15s;
      &:hover { color: var(--primary); border-color: var(--primary); }
    }
  }

  .inspector-tabs {
    display: flex;
    border-bottom: 1px solid var(--border-subtle);
    flex-shrink: 0;
    background: var(--surface-1);

    .inspector-tab {
      flex: 1;
      padding: 0.4rem;
      font-size: 0.7rem;
      font-weight: 600;
      color: var(--text-muted);
      background: transparent;
      border: none;
      border-bottom: 2px solid transparent;
      cursor: pointer;
      transition: all 0.15s;
      &:hover { color: var(--text-secondary); }
      &.active {
        color: var(--primary);
        border-bottom-color: var(--primary);
        background: rgba(59, 130, 246, 0.05);
      }
    }
  }

  .inspector-content {
    flex: 1;
    overflow-y: auto;
    padding: 0.75rem;
    &::-webkit-scrollbar { width: var(--scroll-bar-width); height: var(--scroll-bar-width); }
    &::-webkit-scrollbar-track {
      border-radius: var(--curve-factor);
      background-color: var(--scroll-bar-background);
    }
    &::-webkit-scrollbar-thumb {
      background: var(--scroll-bar-color);
      border-radius: var(--curve-factor);
    }
  }

  .inspector-group {
    margin-bottom: 1rem;
    .inspector-label {
      display: block;
      font-size: 0.7rem;
      font-weight: 700;
      color: var(--text-muted);
      text-transform: uppercase;
      letter-spacing: 0.05em;
      margin-bottom: 0.35rem;
    }
    .inspector-select {
      width: 100%;
      padding: 0.35rem 0.5rem;
      font-size: 0.8rem;
      background: var(--surface-2);
      border: 1px solid var(--border-subtle);
      border-radius: var(--curve-factor-small);
      color: var(--text-primary);
      cursor: pointer;
    }
    .inspector-range {
      width: calc(100% - 2rem);
      accent-color: var(--primary);
    }
    .inspector-color {
      width: 1.5rem;
      height: 1.5rem;
      border: 1px solid var(--border-subtle);
      border-radius: var(--curve-factor-small);
      cursor: pointer;
      background: none;
    }
    .inspector-color-row {
      display: flex;
      align-items: center;
      justify-content: space-between;
      font-size: 0.75rem;
      color: var(--text-secondary);
      margin-bottom: 0.25rem;
    }
    .inspector-textarea {
      width: 100%;
      height: 80px;
      padding: 0.5rem;
      font-size: 0.75rem;
      font-family: var(--font-monospace);
      background: var(--surface-2);
      border: 1px solid var(--border-subtle);
      border-radius: var(--curve-factor-small);
      color: var(--text-primary);
      resize: vertical;
    }
    .inspector-code-block {
      font-family: var(--font-monospace);
      font-size: 0.7rem;
      color: var(--text-secondary);
      background: var(--surface-2);
      padding: 0.5rem;
      border-radius: var(--curve-factor-small);
      border: 1px solid var(--border-subtle);
      line-height: 1.5;
    }
  }

  @include tablet {
    width: 260px;
  }
  @include phone {
    position: fixed;
    left: 0; right: 0; bottom: 0;
    top: auto;
    width: 100%;
    height: 50vh;
    border-left: none;
    border-top: 1px solid var(--border-subtle);
    border-radius: var(--curve-factor) var(--curve-factor) 0 0;
  }
}
</style>
