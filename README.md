Here’s a clean way to build a full **Tiptap → PDF download flow using `@react-pdf/renderer`**, with:

* a download button inside your editor UI
* a **mapper** from `jsonContent`
* and **PDF export support**

---

## ✅ Step-by-Step Setup

---

### 1. **Install required packages**

```bash
npm install @react-pdf/renderer
```

---

### 2. **Create a PDF Document Component from Tiptap JSON**

Create a file `TiptapPDFDocument.jsx`:

```jsx
import React from 'react';
import { Document, Page, Text, View, StyleSheet } from '@react-pdf/renderer';

const styles = StyleSheet.create({
  page: { padding: 40 },
  paragraph: { marginBottom: 10, fontSize: 12 },
  bold: { fontWeight: 'bold' },
  italic: { fontStyle: 'italic' },
  heading: { fontSize: 18, marginBottom: 10, fontWeight: 'bold' },
});

const renderTextWithMarks = (node) => {
  let style = {};
  node.marks?.forEach((mark) => {
    if (mark.type === 'bold') style = { ...style, ...styles.bold };
    if (mark.type === 'italic') style = { ...style, ...styles.italic };
  });

  return (
    <Text key={Math.random()} style={style}>
      {node.text}
    </Text>
  );
};

const TiptapPDFDocument = ({ content }) => (
  <Document>
    <Page size="A4" style={styles.page}>
      {content?.content?.map((node, idx) => {
        if (node.type === 'heading') {
          return (
            <Text key={idx} style={styles.heading}>
              {node.content?.map(renderTextWithMarks)}
            </Text>
          );
        }

        if (node.type === 'paragraph') {
          return (
            <View key={idx} style={styles.paragraph}>
              {node.content?.map(renderTextWithMarks)}
            </View>
          );
        }

        // Extend with bullet lists, images, etc., as needed
        return null;
      })}
    </Page>
  </Document>
);

export default TiptapPDFDocument;
```

---

### 3. **Use the Download Button Inside the Editor UI**

Inside your editor component:

```jsx
import React from 'react';
import { useEditor, EditorContent } from '@tiptap/react';
import StarterKit from '@tiptap/starter-kit';
import { PDFDownloadLink } from '@react-pdf/renderer';
import TiptapPDFDocument from './TiptapPDFDocument';

const MyEditor = () => {
  const editor = useEditor({
    extensions: [StarterKit],
    content: '<p>Hello world!</p>',
  });

  return (
    <div>
      <EditorContent editor={editor} />

      {editor && (
        <div style={{ marginTop: 20 }}>
          <PDFDownloadLink
            document={<TiptapPDFDocument content={editor.getJSON()} />}
            fileName="tiptap-export.pdf"
          >
            {({ loading }) => (
              <button disabled={loading}>
                {loading ? 'Preparing PDF...' : 'Download as PDF'}
              </button>
            )}
          </PDFDownloadLink>
        </div>
      )}
    </div>
  );
};

export default MyEditor;
```

---

### ✅ Optional Enhancements

* Add support for:

  * `bullet_list` / `list_item`
  * `code_block`
  * `image` (needs `<Image />` with remote or base64 support)
* Add a "Preview PDF" modal using `BlobProvider` from `@react-pdf/renderer`

---

Would you like a bullet list or image support example as well?
