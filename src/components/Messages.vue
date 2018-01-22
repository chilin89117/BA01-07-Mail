<template>
  <table v-if="msgs.length > 0" class="table table-inbox table-hover">
    <tbody>
      <tr v-for="msg in msgs"
          :class="{unread: typeof msg.isRead !== 'undefined' && !msg.isRead}"
          @click="openMsg(msg)">
        <td><input type="checkbox"></td>
        <td>
          <a href="#" v-if="typeof msg.isImportant !== 'undefined'"
                      @click.prevent.stop="msg.isImportant = !msg.isImportant">
            <i :class="['fa', 'fa-star', {important: msg.isImportant}]"></i>
          </a>
        </td>
        <td>{{msg.from.name}}</td>
        <td>{{msg.subject}}</td>
        <td>
          <i v-if="msg.attachments.length > 0" class="fa fa-paperclip"></i>
        </td>
        <td class="text-right">{{msg.date.fromNow()}}</td>
      </tr>
    </tbody>
  </table>

  <p v-else>No messages here yet.</p>
</template>

<script>
  import {eventBus} from '../main.js';

  export default {
    props: {
      msgs: {
        type: Array,
        required: true,
      },
    },
    methods: {
      openMsg(msg) {
        eventBus.$emit('changeView', {
          tag: 'app-view-msg',
          title: msg.subject,
          data: {msg},
        });
      },
    },
  };
</script>
