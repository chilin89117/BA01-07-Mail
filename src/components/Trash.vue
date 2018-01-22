<template>
  <div class="inbox-body">
    <app-messages :msgs="trashedMsgs"></app-messages>
  </div>
</template>

<script>
import Messages from './Messages.vue';

export default {
  props: {
    data: {
      type: Object,
      required: true,
    },
  },
  computed: {
    trashedMsgs() {
      return this.data.msgs.filter((msg) => {
        return (msg.isDeleted);
      });
    },
  },
  components: {
    appMessages: Messages,
  },
};
</script>
