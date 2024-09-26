<template>
    <editor-content :editor="editor" />
</template>

<script setup lang="ts">
import { useEditor, EditorContent } from '@tiptap/vue-3'
import { Editor } from '@tiptap/core'
import StarterKit from '@tiptap/starter-kit'
import { defineComponent, defineProps, defineEmits } from 'vue'
defineComponent({
    name: 'SparrowTiptap',
})
const props = defineProps({
    content: {
        type: String,
        default: '',
    },
    extensions: {
        type: Array,
        default: () => [],
    },
    placeholder: {
        type: String,
        default: '',
    },
    output: {
        type: String,
        default: 'html',
        validator(output: string): boolean {
            return ['html', 'json'].includes(output)
        },
    },
})

const emit = defineEmits<{
    (e: 'update:content', output: string | object): void
    (e: 'onUpdate', payload: string | object, editor: Editor): void
    (e: 'onCreate', payload: { editor: Editor }): void
    (e: 'onTransaction', payload: { editor: Editor }): void
    (e: 'onFocus', payload: { editor: Editor }): void
    (e: 'onBlur', payload: { editor: Editor }): void
    (e: 'onDestroy', payload: void): void
}>()
const onUpdate = ({ editor }: { editor: Editor }) => {
    let output
    if (props.output === 'html') {
        output = editor.getHTML()
    } else {
        output = editor.getJSON()
    }

    emit('update:content', output)

    emit('onUpdate', output, editor)
}
const editor = useEditor({
    content: props.content,
    extensions: [StarterKit],
    onCreate: (options) => {
        emit('onCreate', options)
    },
    onTransaction: (options) => {
        emit('onTransaction', options)
    },
    onFocus: (options) => {
        emit('onFocus', options)
    },
    onBlur: (options) => {
        emit('onBlur', options)
    },
    onDestroy: (options) => {
        emit('onDestroy', options)
    },
    onUpdate,
})
</script>
<style>
@import '@/style/tiptap.css'
</style>
