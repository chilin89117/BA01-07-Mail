<template>
  <div>
    <a href="#composeModal" data-toggle="modal" class="btn btn-compose">Compose</a>

    <div aria-hidden="true" role="dialog" tabindex="-1" id="composeModal" class="modal fade" style="display: none;">
      <div class="modal-dialog">
        <div class="modal-content">
          <div class="modal-header">
            <button aria-hidden="true" data-dismiss="modal" class="close" type="button">&times;</button>
            <h4 class="modal-title">New Message</h4>
          </div>

          <div class="modal-body">
            <form @submit.prevent="sendMsg" role="form" class="form-horizontal">
              <div class="form-group">
                <label class="col-md-2 control-label" for="subject">Subject</label>
                <div class="col-md-10">
                  <input type="text" v-model="msg.subject" id="subject" class="form-control">
                </div>
              </div>

              <div class="form-group">
                <label class="col-md-2 control-label" for="msg">Message</label>
                <div class="col-md-10">
                  <textarea v-model="msg.content" rows="10" cols="30" class="form-control" id="msg"></textarea>
                </div>
              </div>

              <div class="form-group">
                <div class="col-md-offset-2 col-md-10">
                  <button class="btn btn-primary" type="submit">Send</button>
                  <button class="btn btn-default" data-dismiss="modal">Close</button>
                </div>
              </div>
            </form>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
  import moment from 'moment';
  import {eventBus} from '../main.js';
  export default {
    data() {
      return {
        msg: {
          subject: '',
          content: '',
        },
      };
    },
    methods: {
      sendMsg() {
        eventBus.$emit('sentMsg', {
          msg: {
            subject: this.msg.subject,
            content: this.msg.content,
            isDeleted: false,
            type: 'outgoing',
            date: moment(),
            from: {
              name: 'Chi Lin',
              email: 'chi@example.com',
            },
            attachments: [],
          },
        });
      },
    },
  };
</script>
