<template>
  <section>

    <b-field label="heading">
      <b-input v-model="heading"/>
    </b-field>

    <b-field label="topics">
      <b-taginput
        v-model="topics"
        icon="label"
        placeholder="Add a topic"/>
    </b-field>

    <b-field label="message">
      <!-- <b-input
        v-model="message"
        maxlength="5000"
        type="textarea"/> -->
      <div id="editSection"/>
    </b-field>
    <p class="level">
      <button
        class="button"
        @click="addPost(heading, (editor.getValue()), topics, promoted)">Add New Post</button>
      <b-switch v-model="promoted">
        Promoted
      </b-switch>
    </p>
  </section>
</template>

<script>
import { db, storage } from '../../main.js'

require('codemirror/lib/codemirror.css') // codemirror
require('tui-editor/dist/tui-editor.css') // editor ui
require('tui-editor/dist/tui-editor-contents.css') // editor content
require('tui-color-picker/dist/tui-color-picker.css') // color picker
require('tui-chart/dist/tui-chart.css') // chart
require('highlight.js/styles/github.css') // code block highlight
var Editor = require('tui-editor')
require('tui-editor/dist/tui-editor-extUML.js') // extensions
require('tui-editor/dist/tui-editor-extChart.js')
require('tui-editor/dist/tui-editor-extTable.js')
require('tui-editor/dist/tui-editor-extColorSyntax.js')
require('tui-editor/dist/tui-editor-extScrollSync.js')
var _ = require('lodash')

const uuidv1 = require('uuid/v1')
const fileRegex = /\.[^.\s]+$/i

export default {
  data () {
    return {
      heading: '',
      message: '',
      topics: [],
      promoted: false,
      editor: {}
    }
  },
  computed: {
    imageCache: function () {
      return this.$store.getters.imageCache
    }
  },
  mounted () {
    this.editor = new Editor({
      el: document.querySelector('#editSection'),
      initialEditType: 'wysiwyg',
      previewStyle: 'vertical',
      usageStatistics: 'false',
      minHeight: '300px',
      height: 'auto',
      exts: ['scrollSync', 'colorSyntax', 'uml', 'chart', 'mark', 'table', 'taskCounter'],
      useCommandShortcut: true,
      hooks: {
        'addImageBlobHook': (file, callback) => {
          this.$store.dispatch('addImageToCache', { file, callback })
        }
      }
    })
  },
  methods: {
    addPost (heading, message, topics, promoted) { // <-- and here
      const createdAt = new Date()
      for(let image in this.imageCache){
        if(!this.imageCache[image].storagePath) {
          console.log("All images not yet uploaded PLACEHOLDER")
          return
        }
        message = message.replace(this.imageCache[image].blobPath, this.imageCache[image].storagePath)
      }
      this.$store.dispatch('addPost', { createdAt, heading, message, topics, promoted, vm: this })
    }
  },
  destroyed () {
    this.$store.dispatch('clearImageCache')
  } 
}

</script>

<style lang="scss">

</style>
