<template>
  <aside class="sm-side">
    <div class="user-head">
      <img src="src/assets/images/profile.jpg">
      <div class="user-name">
        <h5>Chi Lin</h5>
        <span class="email-address">info@example.com</span>
      </div>
    </div>

    <div class="compose-wrapper">
      <app-compose></app-compose>
    </div>

    <ul class="inbox-nav">
      <li :class="{active: activeView=='app-inbox'}">
        <a href="#" @click.prevent="navigate('app-inbox', 'Inbox')">
          <i class="fa fa-inbox"></i>Inbox
          <span class="label label-danger pull-right">{{unreadMsgs.length}}</span>
        </a>
      </li>
      <li :class="{active: activeView=='app-sent'}">
        <a href="#" @click.prevent="navigate('app-sent', 'Sent')">
          <i class="fa fa-envelope-o"></i>Sent
          <span class="label label-default pull-right">{{sentMsgs.length}}</span>
        </a>
      </li>
      <li :class="{active: activeView=='app-important'}">
        <a href="#" @click.prevent="navigate('app-important', 'Important')">
          <i class="fa fa-bookmark-o"></i>Important
          <span class="label label-warning pull-right">{{importantMsgs.length}}</span>
        </a>
      </li>
      <li :class="{active: activeView=='app-trash'}">
        <a href="#" @click.prevent="navigate('app-trash', 'Trash')">
          <i class="fa fa-trash-o"></i>Trash
          <span class="label label-default pull-right">{{trashedMsgs.length}}</span>
        </a>
      </li>
    </ul>
  </aside>
</template>

<script>
  import {eventBus} from '../main.js';
  import Compose from './Compose.vue';

  export default {
    created() {
      // Listen for event when view is changed to assign 'active' class
      eventBus.$on('changeView', (data) => {
        this.activeView = data.tag;
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
        activeView: 'app-inbox',
      };
    },
    methods: {
      // Emit an event to Content.vue when new view is selected
      navigate(newView, title) {
        eventBus.$emit('changeView', {
          tag: newView,
          title: title,
        });
      },
    },
    computed: {
      unreadMsgs() {
        return this.msgs.filter((msg) => {
          return (msg.type=='incoming' && !msg.isRead && !msg.isDeleted);
        });
      },
      sentMsgs() {
        return this.msgs.filter((msg) => {
          return (msg.type=='outgoing' && !msg.isDeleted);
        });
      },
      importantMsgs() {
        return this.msgs.filter((msg) => {
          return (msg.type=='incoming' && msg.isImportant && !msg.isDeleted);
        });
      },
      trashedMsgs() {
        return this.msgs.filter((msg) => {
          return (msg.isDeleted);
        });
      },
    },
    components: {
      appCompose: Compose,
    },
  };
</script>
