<template>
  <div class="container">
    <div>
      <Logo />
      <h1 class="title">{{ title }}</h1>
      <p v-if="description" class="subtitle">{{ description }}</p>

      <ul>
        <li>Headers: {{ headers }}</li>
        <li>userAgent: <strong>{{ ua.userAgent }}</strong></li>
        <li>isWebview: <strong>{{ ua.isWebview }}</strong></li>
        <li>isAndroid: <strong>{{ ua.isAndroid }}</strong></li>
        <li>isIOS: <strong>{{ ua.isIOS }}</strong></li>
        <li>isIOSStandalone: <strong>{{ ua.isIOSStandalone }}</strong></li>
        <li>isIframe: <strong>{{ ua.isIframe }}</strong></li>
      </ul>
    </div>
  </div>
</template>

<script>
import isWebview from 'is-ua-webview';

export default {
  asyncData(context) {
    const requestHeaders = (process.server) ? context.req.headers['user-agent'] : null;

    return {
      title: 'This is Nuxt',
      description: 'and this is builder..',
      headers: requestHeaders,
    };
  },

  data() {
    return {
      title: 'Nuxt.js Sandbox',
      description: '',
      headers: null,
      ua: {
        isWebview: null,
        isIframe: null,
        userAgent: null,
        isIOS: null,
        isIOSStandalone: null,
        isAndroid: null,
      },
    };
  },

  mounted() {
    this.init();
  },

  methods: {
    init() {
      this.ua = {
        isWebview: this.isWebview(),
        isIframe: this.isIframe(),
        userAgent: this.userAgent(),
        isIOS: this.isIOS(),
        isIOSStandalone: this.isIOSStandalone(),
        isAndroid: this.isAndroid(),
      };
    },

    isWebview() {
      return isWebview(navigator.userAgent);
      // return (webview || this.isAndroid || this.isIOS);
    },

    isIframe() {
      try {
        return window.self !== window.top;
      } catch (e) {
        return true;
      }
    },

    userAgent() {
      // if (!process.server) return '';
      return navigator.userAgent.toLowerCase();
    },

    isIOSStandalone() {
      const ios = /iphone|ipod|ipad/.test(this.userAgent);
      const safari = /safari/.test(this.userAgent);
      if (ios && !safari) return true;
      return false;
    },

    isIOS() {
      const { standalone } = navigator.userAgent;
      const safari = /safari/.test(this.userAgent);
      const ios = /iphone|ipod|ipad/.test(this.userAgent);
      if (ios && !safari && !standalone) return true;
      return false;
    },

    isAndroid() {
      const android = /android/.test(this.userAgent);
      const wv = /wv/.test(this.userAgent);
      if (android && wv) return true;
      return false;
    },
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
