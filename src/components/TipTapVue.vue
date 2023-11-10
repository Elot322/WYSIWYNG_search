<template>
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
    <EditorContent
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
        <pre>
          {{ jsonModel }}
        </pre>
      </div>
      <div
        :class="$style.containerWithHtml"
        v-html="htmlModel"/>
    </div>
  </div>
</template>

<script lang="ts">
import { useEditor, EditorContent } from '@tiptap/vue-3'
import StarterKit from '@tiptap/starter-kit'
import TextAlign from '@tiptap/extension-text-align'

import { computed, ref, defineComponent } from 'vue'

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
        TextAlign.configure({
          types: ['heading', 'paragraph'],
        }),
      ]
    }))

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
      textAlignRight
    }
  }
})
</script>

<style module>
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
  border: solid black;
}
</style>