<template>
  <div class="sc-chat-window-container">
    <ChatWindow
      :messageList="messageList"
      :onUserInputSubmit="onMessageWasSent"
      :participants="participants"
      :title="chatWindowTitle"
      :titleImageUrl="titleImageUrl"
      :isOpen="isOpen"
      :isEmbedded="isEmbedded"
      :onClose="close"
      :showEmoji="showEmoji"
      :showFile="showFile"
      :placeholder="placeholder"
      :showTypingIndicator="showTypingIndicator"
      :colors="colors"
      :alwaysScrollToBottom="alwaysScrollToBottom"
      :messageStyling="messageStyling"
      :disableUserListToggle="disableUserListToggle"
      @scrollToTop="$emit('scrollToTop')"
      @onType="$emit('onType')"
      @edit="$emit('edit', $event)"
    >
      <template v-slot:header>
        <slot name="header">
        </slot>
      </template>
      <template v-slot:user-avatar="scopedProps">
        <slot name="user-avatar" :user="scopedProps.user" :message="scopedProps.message">
        </slot>
      </template>
      <template v-slot:text-message-body="scopedProps">
        <slot name="text-message-body" :message="scopedProps.message" :messageText="scopedProps.messageText" :messageColors="scopedProps.messageColors" :me="scopedProps.me">
        </slot>
      </template>
      <template v-slot:system-message-body="scopedProps">
        <slot name="system-message-body" :message="scopedProps.message">
        </slot>
      </template>
      <template v-slot:text-message-toolbox="scopedProps">
        <slot name="text-message-toolbox" :message="scopedProps.message" :me="scopedProps.me">
        </slot>
      </template>
    </ChatWindow>
  </div>
</template>
<script>
import ChatWindow from './ChatWindow.vue'

import CloseIcon from './assets/close-icon.png'
import OpenIcon from './assets/logo-no-bg.svg'

export default {
  props: {
    icons:{
      type: Object,
      required: false,
      default: function () {
        return {
            open: {
              img: OpenIcon,
              name: 'default',
            },
            close: {
              img: CloseIcon,
              name: 'default',
            },
        }
      }
    },
    showEmoji: {
      type: Boolean,
      default: false
    },
    isOpen: {
      type: Boolean,
      required: true,
      default: () => true
    },
    isEmbedded: {
      type: Boolean,
      default: () => true
    },
    open: {
      type: Function,
      required: true
    },
    close: {
      type: Function,
      required: true
    },
    showFile: {
      type: Boolean,
      default: false
    },
    participants: {
      type: Array,
      required: true
    },
    title: {
      type: String,
      default: () => ''
    },
    titleImageUrl: {
      type: String,
      default: () => ''
    },
    onMessageWasSent: {
      type: Function,
      required: true
    },
    messageList: {
      type: Array,
      default: () => []
    },
    newMessagesCount: {
      type: Number,
      default: () => 0
    },
    placeholder: {
      type: String,
      default: 'Write a reply'
    },
    showTypingIndicator: {
      type: String,
      default: () => ''
    },
    colors: {
      type: Object,
      required: false,
      validator: c => 
        'header' in c
        && 'bg' in c.header 
        && 'text' in c.header
        && 'launcher' in c
        && 'bg' in c.launcher
        && 'messageList' in c
        && 'bg' in c.messageList
        && 'sentMessage' in c
        && 'bg' in c.sentMessage 
        && 'text' in c.sentMessage
        && 'receivedMessage' in c
        && 'bg' in c.receivedMessage 
        && 'text' in c.receivedMessage
        && 'userInput' in c
        && 'bg' in c.userInput 
        && 'text' in c.userInput,
      default: function () {
        return {
          header: {
            bg: '#4e8cff',
            text: '#ffffff'
          },
          launcher: {
            bg: '#4e8cff'
          },
          messageList: {
            bg: '#ffffff'
          },
          sentMessage: {
            bg: '#4e8cff',
            text: '#ffffff'
          },
          receivedMessage: {
            bg: '#f4f7f9',
            text: '#ffffff'
          },
          userInput: {
            bg: '#f4f7f9',
            text: '#565867'
          }
        }
      }
    },
    alwaysScrollToBottom: {
      type: Boolean,
      default: () => false
    },
    messageStyling: {
      type: Boolean,
      default: () => false
    },
    disableUserListToggle: {
      type: Boolean,
      default: false
    }
  },
  computed: {
    chatWindowTitle() {
      if (this.title !== '') {
        return this.title
      }

      if (this.participants.length === 0) {
        return 'You'
      } else if (this.participants.length > 1) {
        return 'You, ' + this.participants[0].name + ' & others'
      } else {
        return 'You & ' + this.participants[0].name
      }
    }
  },
  components: {
    ChatWindow
  }
}
</script>
<style scoped>
.sc-new-messsages-count {
  position: absolute;
  top: -3px;
  left: 41px;
  display: flex;
  justify-content: center;
  flex-direction: column;
  border-radius: 50%;
	width: 22px;
  height: 22px;
  background: #ff4646;
  color: white;
  text-align: center;
  margin: auto;
  font-size: 12px;
  font-weight: 500;
}
</style>
