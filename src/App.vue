<template>
  <div class="container">
    <div class="mail-box">
      <app-sidebar :msgs="messages"></app-sidebar>
      <app-content :msgs="messages"></app-content>
    </div>
  </div>
</template>

<script>
  import Sidebar from './components/Sidebar.vue';
  import Content from './components/Content.vue';

  import messages from './data/messages.js';
  import randomMsgs from './data/random-messages.js';

  import {eventBus} from './main.js';

  export default {
    created() {
      eventBus.$on('refreshMsgs', () => {
        let randomIndex = Math.floor(Math.random() * randomMsgs.length);
        this.messages.unshift(randomMsgs[randomIndex]);
      });
      eventBus.$on('sentMsg', (data) => {
        this.messages.unshift(data.msg);
      });
    },
    data() {
      // Keep track of messages globally
      return {
        messages,
      };
    },
    components: {
      appSidebar: Sidebar,
      appContent: Content,
    },
  };
</script>
