<template>
  <div
    :class="$style.info">
    <h1>
      TipTapVue
    </h1>
    <div>
      <div :class="$style.buttonsContainer">
        <button
          :class="{[$style.isActive]: editor?.isActive('bold')}"
          @click="boldClick">
          Bold
        </button>
        <button
          :class="{[$style.isActive]: editor?.isActive('italic')}"
          @click="italicClick">
          Italics
        </button>
        <button
          @click="textAlignLeft">
          TextAlignLeft
        </button>
        <button
          @click="textAlignCenter">
          TextAlignCenter
        </button>
        <button
          @click="textAlignRight">
          TextAlignRight
        </button>
      </div>
      <div :class="$style.buttonsContainer">
        <button
          :class="{[$style.isActive]: editor?.isActive('heading', { level: 1 })}"
          @click="setHeading(1)">
          H1
        </button>
        <button
          :class="{[$style.isActive]: editor?.isActive('heading', { level: 2 })}"
          @click="setHeading(2)">
          H2
        </button>
        <button
          :class="{[$style.isActive]: editor?.isActive('heading', { level: 3 })}"
          @click="setHeading(3)">
          H3
        </button>
      </div>
      <div :class="$style.buttonsContainer">
        <input
          v-model="tableRows"/>
        x
        <input
          v-model="tableColumns"/>
        <button
          @click="addTable">
          addTable
        </button>
      </div>
      <EditorContent
        :class="$style.editor__content"
        :editor="editor"/>
      <button
        @click="onOpenHtmlClick">
        Предпросмотр
      </button>
      <div v-if="isHtmlPreviewOpen">
        <div
          :class="$style.containerWithCode">
          <pre>
            {{ htmlModel }}
          </pre>
        </div>
        <div
          :class="$style.containerWithCode">
            {{ jsonModel }}
        </div>
        <div
          :class="$style.containerWithHtml"
          v-html="htmlModel"/>
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import { computed, ref, defineComponent } from 'vue'
import StarterKit from '@tiptap/starter-kit'
import TextAlign from '@tiptap/extension-text-align'
import Heading from '@tiptap/extension-heading'
import Table from '@tiptap/extension-table'
import TableCell from '@tiptap/extension-table-cell'
import TableHeader from '@tiptap/extension-table-header'
import TableRow from '@tiptap/extension-table-row'
import { useEditor } from '@tiptap/vue-3'

import { EditorContent } from '@tiptap/vue-3'

export default defineComponent({
  name: 'TipTapVueComponent',
  components: {
    EditorContent
  },
  setup() {
    const editor = ref(useEditor({
      content: 'I’m running Tiptap with Vue.js. 🎉',
      extensions: [
        StarterKit,
        Heading,
        TextAlign.configure({
          types: ['heading', 'paragraph'],
        }),
        Table.configure({
          resizable: true,
        }),
        TableRow,
        TableHeader,
        TableCell,
      ]
    }))

    const tableRows = ref(1)
    const tableColumns = ref(1)

    function boldClick() {
      editor.value?.chain().focus().toggleBold().run()
    }

    function italicClick() {
      editor.value?.chain().focus().toggleItalic().run()
    }

    function textAlignLeft() {
      editor.value?.chain().focus().setTextAlign('left').run()
    }

    function textAlignCenter() {
      editor.value?.chain().focus().setTextAlign('center').run()
    }

    function textAlignRight() {
      editor.value?.chain().focus().setTextAlign('right').run()
    }

    function setHeading(value: 1 | 2 | 3 | 4 | 5 | 6) {
      editor.value?.chain().focus().toggleHeading({level: value}).run()
    }

    function addTable() {
      editor.value?.chain().focus().insertTable({rows: tableRows.value, cols: tableRows.value, withHeaderRow: false}).run()
    }

    const jsonModel = computed(() => editor.value?.getJSON())
    const htmlModel = computed(() => editor.value?.getHTML())

    const isHtmlPreviewOpen = ref(false)
    function onOpenHtmlClick() {
      isHtmlPreviewOpen.value = !isHtmlPreviewOpen.value
    }

    return {
      editor,
      boldClick,
      italicClick,
      onOpenHtmlClick,
      isHtmlPreviewOpen,
      jsonModel,
      htmlModel,
      textAlignLeft,
      textAlignCenter,
      textAlignRight,
      setHeading,
      tableRows,
      tableColumns,
      addTable
    }
  }
})
</script>

<style module>

input {
  width: 50px;
}

button {
  background-color:white;
}

pre {
  margin: 10px 0 30px;
  padding: 20px 25px;
  line-height: 20px;
  font-size: 14px;
  border-radius: 8px;
  overflow: auto;
}


.buttonsContainer {
  display: flex;
  flex-direction: row;

  .isActive {
    font-weight: bolder;
    color: white;
    background-color: black;
  }
}

.containerWithCode {
  margin-top: 5px;
  border: solid black;
  background-color: antiquewhite;
  border-radius: 5px;
}

.containerWithHtml {
  margin-top: 5px;
  border: solid black;
  background-color: aliceblue;
  border-radius: 5px;
}

.editorField {
  width: 100%;
}

.editor__content table td, .editor__content table th { min-width: 1em; border: 2px solid #ddd; padding: 3px 5px; vertical-align: top; -webkit-box-sizing: border-box; box-sizing: border-box; position: relative; }
</style>