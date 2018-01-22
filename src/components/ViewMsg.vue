<template>
  <div class="inbox-body">
    <div class="mail-option">
      <button class="btn btn-primary btn-sm" @click="goBack">
        <i class="fa fa-arrow-left"></i>&nbsp;Back
      </button>
      <button class="btn btn-danger btn-sm"
              @click="data.msg.isDeleted=true"
              :disabled="data.msg.isDeleted">
        <i class="fa fa-trash"></i>&nbsp;{{data.msg.isDeleted?'Deleted':'Delete'}}
      </button>
      <template v-if="typeof data.msg.isRead !== 'undefined'">
        <button class="btn btn-default btn-sm"
                @click="data.msg.isRead=false"
                :disabled="!data.msg.isRead">
          <i class="fa fa-envelope"></i>&nbsp;Mark as Unread
        </button>
        <button class="btn btn-success btn-sm"
                @click="data.msg.isRead=true"
                :disabled="data.msg.isRead">
          <i class="fa fa-envelope-open"></i>&nbsp;Mark as Read
        </button>
      </template>
    </div>
    <p><strong>Date:</strong>{{data.msg.date.fromNow()}}</p>
    <p><strong>From:</strong>{{data.msg.from.name}} &lt; {{data.msg.from.email}} &gt;</p>
    <hr>
    <div v-html="data.msg.content" class="message"></div>
    <div v-if="data.msg.attachments.length > 0" class="attachments">
      <h4>Attachments</h4>
      <ul>
        <li v-for="att in data.msg.attachments">
          <i class="fa fa-paperclip"></i>
          {{att.fileName}} ({{att.size | formatBytes}})
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
  import {eventBus} from '../main.js';

  export default {
    props: {
      data: {
        type: Object,
        required: true,
      },
    },
    activated() {
      if(typeof this.data.msg.isRead != 'undefined')
        {this.data.msg.isRead = true;
      }
    },
    methods: {
      goBack() {
        let previous = this.$parent.previousView;
        eventBus.$emit('changeView', previous);
      },
    },
    filters: {
      formatBytes(bytes) {
        if (bytes == 0) {return '0 Bytes';}
        let dm = 2;
        let k = 1024;
        let sizes = ['Bytes', 'KB', 'MB', 'GB', 'TB', 'PB', 'EB', 'ZB', 'YB'];
        let i = Math.floor(Math.log(bytes) / Math.log(k));
        return parseFloat((bytes / Math.pow(k, i)).toFixed(dm)) + ' ' + sizes[i];
      },
    },
  };
</script>
