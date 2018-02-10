# BA01-07-Mail
## Vue.js application using components to simulate e-mail inbox

![Home Page](../assets/a.png?raw=true)

### `App.vue`
- passes messages from `data/messages.js` to `Sidebar` and `Content` components
- listens for `refreshMsgs` event to randomly add a message from `data/random-messages.js`
- listens for `sentMsg` event to add new message to global `messages` variable

### `Sidebar` component
- has links to `Inbox`, `Sent`, `Important`, and `Trash`
  - emits `changeView` event when clicked
- has counts for each type of messages
- has `Compose` component which
  - has a button to open a modal window for creating a new message
  - emits `sentMsg` event on the event bus

### `Content` component
- main component to display child components: `Inbox`, `Sent`, `Important`, `Trash`, `ViewMsg`
- listens for `changeView` event, then loads appropriate component and its messages
- `Inbox` component
  - uses `Messages` component to display all incoming messages
  - emits `changeView` event to display `ViewMsg` component when a message is selected 
    - `ViewMsg` component has buttons to go back, delete, mark as read/unread
  - has a button to refresh by emitting `refreshMsgs` event on the event bus
    
