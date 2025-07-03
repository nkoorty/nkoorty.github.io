To install all free extensions available in Tiptap, you’ll typically install these packages:

✅ Step 1: Core + Starter Kit
bash
Copy
Edit
npm install @tiptap/react @tiptap/starter-kit
✅ Step 2: Install All Free Extensions Individually
Here’s a full list of free official extensions you can install:

bash
Copy
Edit
npm install \
  @tiptap/extension-blockquote \
  @tiptap/extension-bold \
  @tiptap/extension-bullet-list \
  @tiptap/extension-character-count \
  @tiptap/extension-code \
  @tiptap/extension-code-block \
  @tiptap/extension-document \
  @tiptap/extension-dropcursor \
  @tiptap/extension-gapcursor \
  @tiptap/extension-hard-break \
  @tiptap/extension-heading \
  @tiptap/extension-history \
  @tiptap/extension-horizontal-rule \
  @tiptap/extension-italic \
  @tiptap/extension-list-item \
  @tiptap/extension-ordered-list \
  @tiptap/extension-paragraph \
  @tiptap/extension-placeholder \
  @tiptap/extension-strike \
  @tiptap/extension-text \
  @tiptap/extension-underline
✅ Step 3: Add Extensions to the Editor
Example Editor.tsx:

tsx
Copy
Edit
'use client'

import { useEditor, EditorContent } from '@tiptap/react'
import StarterKit from '@tiptap/starter-kit'
import Underline from '@tiptap/extension-underline'
import Placeholder from '@tiptap/extension-placeholder'
import CharacterCount from '@tiptap/extension-character-count'

export default function Editor() {
  const editor = useEditor({
    extensions: [
      StarterKit,
      Underline,
      Placeholder.configure({
        placeholder: 'Start typing...',
      }),
      CharacterCount.configure({
        limit: 1000,
      }),
    ],
    content: '<p>Hello World!</p>',
  })

  return (
    <div>
      <EditorContent editor={editor} />
      <p>Character count: {editor?.storage.characterCount.characters()}</p>
    </div>
  )
}
Notes
StarterKit already includes many extensions: Paragraph, Text, Heading, Bold, Italic, Strike, List, Code, Blockquote, History, etc.

If you add those extensions again manually, they’ll override StarterKit's config.

Paid extensions like collaboration, table resizing, or AI suggestions are only available via Tiptap’s Pro license.

Let me know if you want a list of paid vs free extensions or help building a full WYSIWYG with buttons.

You said:
I want to see the toolbar with the editor, add all extensions for h1-h whatever and all the standard shit
ChatGPT said:
Got it. Here’s a full working WYSIWYG editor setup in Next.js + TypeScript using Tiptap, with:

Toolbar (Bold, Italic, Underline, Strike, Headings h1–h6, Lists, Code, Blockquote, etc.)

All free extensions you need

Clean formatting and ready to use

✅ 1. Install All Dependencies
bash
Copy
Edit
npm install @tiptap/react @tiptap/starter-kit \
@tiptap/extension-underline \
@tiptap/extension-placeholder \
@tiptap/extension-character-count \
@tiptap/extension-text-align
✅ 2. Create Editor.tsx
tsx
Copy
Edit
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
✅ 3. Use It in Your Page
tsx
Copy
Edit
import Editor from '@/components/Editor'

export default function Page() {
  return (
    <div className="max-w-3xl mx-auto p-8">
      <Editor />
    </div>
  )
}
✅ 4. Add Styling
Update your global CSS (e.g. globals.css):

css
Copy
Edit
.ProseMirror {
  outline: none;
  min-height: 200px;
}
Next Steps (Optional)
Let me know if you want to add:

Image upload

Table support

Markdown export

Persistence via Supabase / API

I can give you those too.

You said:
Use the the TipTap documentation to do this correctly just like tip tap wants you to
ChatGPT said:
Here’s a clean, docs-aligned implementation using the official Tiptap guide for Next.js, featuring a toolbar, StarterKit, all standard formatting, and ready-to-use UI components via Tiptap CLI.

🚀 1. Initialise or integrate into your Next.js project
If you’re starting fresh:

bash
Copy
Edit
npx create-next-app my-tiptap-editor
cd my-tiptap-editor
📦 2. Install Tiptap core + UI components
Run:

bash
Copy
Edit
npm install @tiptap/react @tiptap/pm @tiptap/starter-kit
Now scaffold the toolbar UI via Tiptap’s CLI:

bash
Copy
Edit
npx @tiptap/cli init
# Select “Next.js” and choose “UI Components”
Optionally, install individual UI buttons, e.g.:

bash
Copy
Edit
pnpm dlx @tiptap/cli add heading-button
pnpm dlx @tiptap/cli add bold-button
pnpm dlx @tiptap/cli add italic-button
# …and so on for underline, strike, bullet-list, ordered-list, code-block…
This command generates ready-to-import components in your components/ui/ folder 
tiptap.dev
+15
tiptap.dev
+15
dev.to
+15
.

Also, make sure SCSS variables and keyframes are added to your global stylesheet:

scss
Copy
Edit
@import 'tiptap-ui/styles/_variables.scss';
@import 'tiptap-ui/styles/_keyframe-animations.scss';
tiptap.dev
.

🧩 3. Create the Editor component
Create components/Editor.tsx:

tsx
Copy
Edit
'use client'

import { useEditor, EditorContent } from '@tiptap/react'
import StarterKit from '@tiptap/starter-kit'

// UI components generated by the CLI:
import { Toolbar } from '@/components/ui/toolbar'
import { BoldButton } from '@/components/ui/bold-button'
import { ItalicButton } from '@/components/ui/italic-button'
import { UnderlineButton } from '@/components/ui/underline-button'
import { StrikeButton } from '@/components/ui/strike-button'
import { HeadingButton } from '@/components/ui/heading-button'
import { BulletListButton } from '@/components/ui/bullet-list-button'
import { OrderedListButton } from '@/components/ui/ordered-list-button'
import { CodeBlockButton } from '@/components/ui/code-block-button'
import { BlockquoteButton } from '@/components/ui/blockquote-button'
import { UndoButton } from '@/components/ui/undo-button'
import { RedoButton } from '@/components/ui/redo-button'

export default function Editor() {
  const editor = useEditor({
    extensions: [StarterKit.configure({ heading: { levels: [1,2,3,4,5,6] } })],
    content: '<p>Hello, Tiptap!</p>',
  })

  return (
    <div>
      <Toolbar>
        <BoldButton editor={editor} />
        <ItalicButton editor={editor} />
        <UnderlineButton editor={editor} />
        <StrikeButton editor={editor} />
        <HeadingButton editor={editor} level={1}>H1</HeadingButton>
        <HeadingButton editor={editor} level={2}>H2</HeadingButton>
        <HeadingButton editor={editor} level={3}>H3</HeadingButton>
        <BulletListButton editor={editor} />
        <OrderedListButton editor={editor} />
        <BlockquoteButton editor={editor} />
        <CodeBlockButton editor={editor} />
        <UndoButton editor={editor} />
        <RedoButton editor={editor} />
      </Toolbar>

      <div className="border rounded-md p-4 mt-2 min-h-[300px]">
        <EditorContent editor={editor} />
      </div>
    </div>
  )
}
This follows Tiptap's recommended pattern: headless core + UI primitives added via CLI components 
tiptap.dev
+6
github.com
+6
github.com
+6
youtube.com
+15
tiptap.dev
+15
tiptap.dev
+15
.

🧭 4. Use the Editor in your page
In pages/index.tsx or app/page.tsx:

tsx
Copy
Edit
import Editor from '@/components/Editor'

export default function Home() {
  return (
    <main className="max-w-3xl mx-auto p-8">
      <h1 className="text-2xl font-bold mb-4">Tiptap WYSIWYG 🌟</h1>
      <Editor />
    </main>
  )
}
🎨 5. Import styles
Ensure your global stylesheet imports the styles needed by your UI components:

css
Copy
Edit
@import 'tiptap-ui/styles/_variables.scss';
@import 'tiptap-ui/styles/_keyframe-animations.scss';
If you're using SCSS, adjust accordingly 
liveblocks.io
+2
tiptap.dev
+2
ckriswinarto.medium.com
+2
tiptap.dev
.

✅ Summary
You now have a robust WYSIWYG editor built exactly as Tiptap suggests:

StarterKit for all standard formatting nodes + marks 
github.com
+15
tiptap.dev
+15
dev.to
+15

Toolbar buttons for bold, italic, headings H1–H3 (or up to H6), lists, code blocks, blockquote, undo/redo

Official UI components via Tiptap CLI to maintain consistency and future compatibility 
ckriswinarto.medium.com
+2
tiptap.dev
+2
tiptap.dev
+2

🔧 What's next?
Add more UI components: link picker, image uploader, text alignment, etc.

Customise styling with Tailwind or CSS modules.

Hook into onUpdate(), .getHTML() or .getJSON() for data storage or API integration.

Let me know which extra features you'd like to add next!
