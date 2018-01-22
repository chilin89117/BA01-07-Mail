<template>
  <div class="inbox-body">
    <app-messages :msgs="sentMsgs"></app-messages>
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
      sentMsgs() {
        return this.data.msgs.filter((msg) => {
          return (msg.type=='outgoing' && !msg.isDeleted);
        });
      },
    },
    components: {
      appMessages: Messages,
    },
  };
</script>
