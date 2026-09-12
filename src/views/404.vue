<template>
  <main class="not-found-page">
    <h1 class="not-found-title">404</h1>
    <h2 class="not-found-sad-face">:(</h2>
    <p class="not-found-subtitle">Page Not Found</p>
    <p class="not-found-message">
      Facing Issues?
      <a href="https://git.io/JzpL5">Get Support</a>.
    </p>
    <router-link to="/" class="go-home">Back Home</router-link>
  </main>
</template>

<script>

export default {
  name: 'not-found',
  methods: {
    setTheme() {
      document.getElementsByTagName('html')[0].setAttribute('data-theme', 'shipyard-docs');
    },
  },
  mounted() {
    this.setTheme();
  },
};
</script>

<style scoped lang="scss">
@import '@/styles/media-queries.scss';
@import '@/styles/style-helpers.scss';
main.not-found-page {
  margin: 0;
  padding: 0;
  width: 100%;
  height: 100%;
  display: flex;
  flex-direction: column;
  justify-content: flex-start;
  align-items: center;
  cursor: default;
  background: var(--background-darker);
  min-height: calc(99vh - var(--footer-height));
  h1.not-found-title, h2.not-found-sad-face {
    font-size: 20vh;
    font-family: Tahoma, monospace;
    cursor: default;
    color: var(--text-primary);
    text-shadow: 0 2px 4px rgba(0, 0, 0, 0.5);
    margin: 1rem 0 0;
  }
  h2.not-found-sad-face {
    font-size: 4rem;
    margin: 0 0 1.5rem 0;
  }
  p {
    font-family: monospace;
    cursor: default;
    color: var(--text-secondary);
    margin: 0.2rem 0;
    text-shadow: 0 1px 2px rgba(0, 0, 0, 0.5);
  }
  p.not-found-subtitle {
    font-size: 2.8rem;
  }
  p.not-found-message {
    font-size: 1.4rem;
    font-weight: normal;
    a {
      color: var(--primary);
      font-family: monospace;
    }
  }
  a.go-home {
    padding: 0.3rem 1rem;
    border-radius: var(--curve-factor);
    font-size: 1.7rem;
    cursor: pointer;
    font-family: Tahoma, monospace;
    color: var(--background);
    margin: 2rem 0 0;
    text-decoration: none;
    background: var(--primary);
    box-shadow: 0 4px var(--accent-glow);
    &:hover { box-shadow: 0 2px var(--accent-glow); }
  }
  ::selection { background-color: var(--primary); color: var(--background); }
}
</style>
