<template>
  <div class="container">
    <div>
      <Logo />
      <h1 class="title">{{ title }}</h1>
      <p v-if="description" class="subtitle">{{ description }}</p>
    </div>
  </div>
</template>

<script>
export default {
  asyncData() {
    return {
      title: 'This is Nuxt',
      description: 'and this is madness..',
    };
  },

  data() {
    return {
      title: 'Nuxt.js Sandbox',
      description: '',
    };
  },
};
</script>

<style>
.container {
  margin: 0 auto;
  min-height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  text-align: center;
}

.title {
  font-family:
    'Quicksand',
    'Source Sans Pro',
    -apple-system,
    BlinkMacSystemFont,
    'Segoe UI',
    Roboto,
    'Helvetica Neue',
    Arial,
    sans-serif;
  display: block;
  margin: 1rem 0;
  font-weight: 300;
  font-size: 3rem;
  color: #35495e;
  letter-spacing: 1px;
}

.subtitle {
  font-weight: 300;
  font-size: 1.5rem;
  color: #526488;
  word-spacing: 5px;
  padding-bottom: 15px;
}
</style>
