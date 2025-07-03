'use client'

import React from 'react'
import { useEditor, EditorContent } from '@tiptap/react'
import StarterKit from '@tiptap/starter-kit'
import Underline from '@tiptap/extension-underline'
import Placeholder from '@tiptap/extension-placeholder'
import CharacterCount from '@tiptap/extension-character-count'
import TextAlign from '@tiptap/extension-text-align'

const MenuBar = ({ editor }: { editor: any }) => {
  if (!editor) return null

  return (
    <div className="flex gap-2 mb-2 flex-wrap text-sm">
      <button onClick={() => editor.chain().focus().toggleBold().run()} className={editor.isActive('bold') ? 'font-bold' : ''}>Bold</button>
      <button onClick={() => editor.chain().focus().toggleItalic().run()} className={editor.isActive('italic') ? 'italic' : ''}>Italic</button>
      <button onClick={() => editor.chain().focus().toggleUnderline().run()} className={editor.isActive('underline') ? 'underline' : ''}>Underline</button>
      <button onClick={() => editor.chain().focus().toggleStrike().run()} className={editor.isActive('strike') ? 'line-through' : ''}>Strike</button>

      {[1, 2, 3, 4, 5, 6].map(level => (
        <button
          key={level}
          onClick={() => editor.chain().focus().toggleHeading({ level }).run()}
          className={editor.isActive('heading', { level }) ? 'font-bold' : ''}
        >
          H{level}
        </button>
      ))}

      <button onClick={() => editor.chain().focus().toggleBulletList().run()}>• List</button>
      <button onClick={() => editor.chain().focus().toggleOrderedList().run()}>1. List</button>
      <button onClick={() => editor.chain().focus().toggleBlockquote().run()}>“ Quote</button>
      <button onClick={() => editor.chain().focus().toggleCodeBlock().run()}>Code</button>
      <button onClick={() => editor.chain().focus().setParagraph().run()}>Paragraph</button>
      <button onClick={() => editor.chain().focus().unsetAllMarks().run()}>Clear</button>
    </div>
  )
}

export default function Editor() {
  const editor = useEditor({
    extensions: [
      StarterKit.configure({ heading: { levels: [1, 2, 3, 4, 5, 6] } }),
      Underline,
      Placeholder.configure({ placeholder: 'Write something...' }),
      CharacterCount.configure({ limit: 10000 }),
      TextAlign.configure({ types: ['heading', 'paragraph'] }),
    ],
    content: '<p>Hello World</p>',
  })

  return (
    <div className="border rounded-md p-4">
      <MenuBar editor={editor} />
      <div className="border p-2 min-h-[200px]">
        <EditorContent editor={editor} />
      </div>
      <p className="text-xs text-right mt-2 text-gray-500">
        {editor?.storage.characterCount.characters()} characters
      </p>
    </div>
  )
}
