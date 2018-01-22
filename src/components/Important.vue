<template>
  <div class="inbox-body">
    <app-messages :msgs="importantMsgs"></app-messages>
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
      importantMsgs() {
        return this.data.msgs.filter((msg) => {
          return (msg.type=='incoming' && msg.isImportant && !msg.isDeleted);
        });
      },
    },
    components: {
      appMessages: Messages,
    },
  };
</script>
