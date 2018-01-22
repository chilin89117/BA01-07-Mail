<template>
  <aside class="lg-side">
    <div class="inbox-head">
      <h3>{{currentView.title}}</h3>
    </div>
    <keep-alive>
      <component :is="currentView.tag" :data="currentView.data"></component>
    </keep-alive>
  </aside>
</template>

<script>
import Inbox from './Inbox.vue';
import Sent from './Sent.vue';
import Important from './Important.vue';
import Trash from './Trash.vue';
import ViewMsg from './ViewMsg.vue';

import {eventBus} from '../main.js';

export default {
  created() {
    // Listen for event when sidebar item is clicked to change component
    eventBus.$on('changeView', (data) => {
      let temp = {
        tag: data.tag,
        title: data.title,
        data: data.data || {},
      };
      // Add the new view as 1st item in history array
      this.history.unshift(temp);
    });
  },
  props: {
    msgs: {
      type: Array,
      required: true,
    },
  },
  data() {
    return {
      history: [
        {
          tag: 'app-inbox',
          title: 'Inbox',
          data: {
            msgs: null,
          },
        }
      ],
    };
  },
  computed: {
    currentView() {
      let current = this.history[0];
      current.data.msgs = this.msgs;
      return current;
    },
    previousView() {
      return typeof this.history[1] !== 'undefined' ? this.history[1] : null;
    },
  },
  components: {
    appInbox: Inbox,
    appSent: Sent,
    appImportant: Important,
    appTrash: Trash,
    appViewMsg: ViewMsg,
  },
};
</script>
