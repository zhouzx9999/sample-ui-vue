<!-- 
- Message.vue -
エラーメッセージ表示機能を提供する Vue コンポーネント。
グローバルイベント(Event.Messages)で通知された内容を表示します。

[template]
Message(global=true)
Message(field=anyMessageKey)
  <input type="text" …
-->
<style lang="sass">
.l-message-group {
  display: block;
  input, textarea, select {
    border-color: #ed4c4c;
    background-color: #f6e3e3;
  }
  .l-message-group-item {
    font-size: 80%;
    padding: .2em;
    margin-bottom: .2em;
  }
}
</style>

<template lang="pug">
div
  div(v-if="global")
    .alert(:class="[classAlert, classText]" v-text="message" v-if="message")
  div(v-if="!global")
    div(:class="{'input-group': message, 'l-message-group': message}")
      slot
      .l-message-group-item.text-danger(v-text="message" v-if="message")
</template>

<script lang="babel">
import {Level, Event} from 'constants'
export default {
  name: 'message',
  data() {
    return {
      classAlert: null,
      classText: null,
      message: null
    }
  },
  props: {
    // グローバル例外表示フラグ
    global: {type: Boolean, default: false},
    // フィールド例外表示キー (グローバル例外表示フラグが false 時に有効)
    field: {type: String}
  },
  created() {
    EventEmitter.$on(Event.Messages, (v) => this.handleMessages(v))
  },
  methods: {
    handleMessages(messages) {
      this.global ? this.handleGlobalMessage(messages) : this.handleColumnMessage(messages)
    },
    handleGlobalMessage(messages) {
      let message = messages.global
      this.message = message
      if (message) {
        let type = this.messageType(messages.level)
        this.classAlert = `alert-${type}`
        this.classText = `text-${type}`
      } else {
        this.classAlert = null
        this.classText = null
      }
    },
    messageType(level) {
      switch (level) {
        case Level.INFO:
          return "success"
        case Level.WARN:
          return "warning"
        case Level.ERROR:
          return "danger"
        default:
          return "default"
      }
    },
    handleColumnMessage(messages) {
      this.message = this.columnError(messages)
    },
    columnError(messages) {
      if (messages && messages.columns && 0 < messages.columns.length) {
        let err = Array.from(messages.columns).find((err) =>
          (messages.level === Level.WARN || messages.level === Level.ERROR) && err.key === this.field)
        if (err) return err.values[0]
      }
      return null
    }
  }
}
</script>