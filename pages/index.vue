<template>
  <div class="wrapper">
    <div>
      <h1 :class="{ 'noway': !deviceWebview }">
        <template v-if="ua.isWebview && ua.isAndroid">Android Webview</template>
        <template v-else-if="ua.isWebview && ua.isIOS">iOS Webview</template>
        <template v-else>Browser</template>
      </h1>

      <div class="container">
        <div v-if="headers" class="list">
          Server Request Headers
          <span>{{ headers }}</span>
        </div>
        <div class="list">
          Browser User Agent
          <span>{{ ua.userAgent }}</span>
        </div>
        <div class="list">
          isWebview:
          <span :class="{ 'true': ua.isWebview }">
            {{ ua.isWebview }}
          </span>
        </div>
        <div class="list">
          isAndroid Webview:
          <span :class="{ 'true': ua.isAndroid }">
            {{ ua.isAndroid }}
          </span>
        </div>
        <div class="list">
          isIOS Webview:
          <span :class="{ 'true': ua.isIOS }">
            {{ ua.isIOS }}
          </span>
        </div>
        <div class="list">
          isIframe:
          <span :class="{ 'true': ua.isIframe }">
            {{ ua.isIframe }}
          </span>
        </div>
        <div class="list">
          maxTouchPoints:
          <span :class="{ 'true': ua.maxTouchPoints }">
            {{ ua.maxTouchPoints }}
          </span>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import isWebview from 'is-ua-webview';

export default {
  asyncData(context) {
    return {
      headers: (process.server) ? context.req.headers['user-agent'] : null,
    };
  },

  data() {
    return {
      headers: null,
      deviceWebview: false,

      ua: {
        isWebview: null,
        isIframe: null,
        userAgent: null,
        isIOS: null,
        isAndroid: null,
        maxTouchPoints: null,
      },
    };
  },

  mounted() {
    this.init();
  },

  methods: {
    init() {
      this.ua = {
        userAgent: this.userAgent(),
        isWebview: isWebview(navigator.userAgent),
        isIframe: this.isIframe(),
        isAndroid: this.isAndroid(),
        isIOS: this.isIOS(),
        maxTouchPoints: navigator.maxTouchPoints,
      };

      this.deviceWebview = this.ua.isWebview;
    },

    isWebview() {
      return isWebview(navigator.userAgent);
    },

    isIframe() {
      try {
        return window.self !== window.top;
      } catch (e) {
        return true;
      }
    },

    userAgent() {
      return navigator.userAgent.toLowerCase();
    },

    isIOS() {
      const ua = navigator.userAgent.toLowerCase();
      const { standalone } = navigator;
      const safari = /safari/.test(ua);
      const ios = /iphone|ipod|ipad/.test(ua);
      return (ios && !safari && !standalone);
    },

    isAndroid() {
      const ua = navigator.userAgent.toLowerCase();
      const android = /android/.test(ua);
      const wv = /wv/.test(ua);
      return (android && wv);
    },
  },
};
</script>

<style>
.wrapper {
  display: flex;
  justify-content: center;
  align-items: center;
  flex-wrap: wrap;
  min-height: 100vh;
  width: 100vw;
  overflow: hidden;
  background-color: #2c2c54;
  color:  #fff;
}

.container {
  margin: 0 auto;
  width: 90%;
  max-width: 400px;
  /* padding: 2rem; */
  /* border-radius: 1rem;
  background-color: rgba(255, 255, 255, 0.02);
  box-shadow: 0px 0.75rem 1.5rem -0.5rem rgba(0,0,0, 0.25); */
}

h1 {
  display: block;
  margin: 2rem auto;
  line-height: 1;
  text-align: center;
}

h1.noway {
  color:  rgba(255, 255, 255, 0.6);
}

.list {
  margin-bottom: 1.5rem;
  font-size: 13px;
  color:  rgba(255, 255, 255, 0.6);
}

.list:last-child {
  margin-bottom: 0;
}

.list span {
  display: block;
  margin-top: 0.5rem;
  padding: 0.5rem 0.5rem;
  font-family: monospace, monospace;
  color: #fff;
  background-color: rgba(255, 255, 255, 0.1);
  font-weight: bold;
}

.list span.true {
  background-color: #4cd137;
  font-weight: bold;
  color: #000;
}
</style>
