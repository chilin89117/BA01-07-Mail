<template>
  <div class="inbox-body">
    <div class="mail-option">
      <div class="btn-group">
        <a href="#" class="btn" @click="refresh">
          <i class="fa fa-refresh"></i>&nbsp;Refresh
        </a>
      </div>
    </div>
    <app-messages :msgs="incomingMsgs"></app-messages>
  </div>
</template>

<script>
import Messages from './Messages.vue';
import {eventBus} from '../main.js';

export default {
  props: {
    data: {
      type: Object,
      required: true,
    },
  },
  computed: {
    incomingMsgs() {
      return this.data.msgs.filter((msg) => {
        return (msg.type=='incoming' && !msg.isDeleted);
      });
    },
  },
  components: {
    appMessages: Messages,
  },
  methods: {
    refresh() {
      eventBus.$emit('refreshMsgs');
    },
  },
};
</script>
