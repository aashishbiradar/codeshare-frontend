<template>
  <div>
    <h1>Live Code Share</h1>
    <codemirror 
      ref="codeM"
      v-model="newCode"
      :options="cmOption"
      @input="codeChange"
    />
  </div>
</template>

<script>
import { codemirror } from 'vue-codemirror'
// base style
import 'codemirror/lib/codemirror.css'
// theme css
import 'codemirror/theme/base16-dark.css'
// language
import 'codemirror/mode/vue/vue.js'
// active-line.js
import 'codemirror/addon/selection/active-line.js'
// styleSelectedText
import 'codemirror/addon/selection/mark-selection.js'
import 'codemirror/addon/search/searchcursor.js'
// highlightSelectionMatches
import 'codemirror/addon/scroll/annotatescrollbar.js'
import 'codemirror/addon/search/matchesonscrollbar.js'
import 'codemirror/addon/search/searchcursor.js'
import 'codemirror/addon/search/match-highlighter.js'
// keyMap
import 'codemirror/mode/clike/clike.js'
import 'codemirror/addon/edit/matchbrackets.js'
import 'codemirror/addon/comment/comment.js'
import 'codemirror/addon/dialog/dialog.js'
import 'codemirror/addon/dialog/dialog.css'
import 'codemirror/addon/search/searchcursor.js'
import 'codemirror/addon/search/search.js'
import 'codemirror/keymap/sublime.js'
// foldGutter
import 'codemirror/addon/fold/foldgutter.css'
import 'codemirror/addon/fold/brace-fold.js'
import 'codemirror/addon/fold/comment-fold.js'
import 'codemirror/addon/fold/foldcode.js'
import 'codemirror/addon/fold/foldgutter.js'
import 'codemirror/addon/fold/indent-fold.js'
import 'codemirror/addon/fold/markdown-fold.js'
import 'codemirror/addon/fold/xml-fold.js'

export default {
  name: 'Share',
  components: { codemirror },
  data() {
    return {
      isConnected: false,
      newCode: '',
      code: '',
      cmOption: {
        tabSize: 4,
        foldGutter: true,
        styleActiveLine: true,
        lineNumbers: true,
        line: true,
        keyMap: "sublime",
        mode: 'text/x-vue',
        theme: 'base16-dark',
        extraKeys: {
          'F11'(cm) {
            cm.setOption("fullScreen", !cm.getOption("fullScreen"))
          },
          'Esc'(cm) {
            if (cm.getOption("fullScreen")) cm.setOption("fullScreen", false)
          }
        }
      }
    };
  },
  computed: {
    codemirror() {
      return this.$refs.codeM.codemirror
    },
    shareId() {
      return this.$route.params.shareId;
    }
  },
  created() {
  },
  methods: {
    codeChange() {
      if (this.newCode === this.code) return;
      this.code = this.newCode;
      const params = {
        shareId: this.shareId,
        payload: this.code
      };
      this.$socket.emit('codeChange', params);
    },
  },
  sockets: {
    connect() {
      // Fired when the socket connects.
      this.isConnected = true;
    },

    disconnect() {
      this.isConnected = false;
    },

    // Fired when the server sends something on the "messageChannel" channel.
    messageChannel(data) {
      this.socketMessage = data
    }
  },
}
</script>

