Here’s a full **TypeScript (TSX)** version of the Tiptap → PDF export flow, cleanly integrated:

---

### ✅ 1. `TiptapPDFDocument.tsx`

```tsx
import React from 'react';
import {
  Document,
  Page,
  Text,
  View,
  StyleSheet,
} from '@react-pdf/renderer';

const styles = StyleSheet.create({
  page: { padding: 40 },
  paragraph: { marginBottom: 10, fontSize: 12 },
  bold: { fontWeight: 'bold' },
  italic: { fontStyle: 'italic' },
  heading: { fontSize: 18, marginBottom: 10, fontWeight: 'bold' },
});

type TiptapNode = {
  type: string;
  content?: TiptapNode[];
  text?: string;
  marks?: { type: string }[];
};

type TiptapDoc = {
  type: string;
  content: TiptapNode[];
};

const renderTextWithMarks = (node: TiptapNode): React.ReactNode => {
  if (!node.text) return null;

  let textStyle = {};
  node.marks?.forEach((mark) => {
    if (mark.type === 'bold') textStyle = { ...textStyle, ...styles.bold };
    if (mark.type === 'italic') textStyle = { ...textStyle, ...styles.italic };
  });

  return (
    <Text key={Math.random()} style={textStyle}>
      {node.text}
    </Text>
  );
};

const TiptapPDFDocument: React.FC<{ content: TiptapDoc }> = ({ content }) => (
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

        return null;
      })}
    </Page>
  </Document>
);

export default TiptapPDFDocument;
```

---

### ✅ 2. `MyEditor.tsx`

```tsx
import React from 'react';
import { useEditor, EditorContent } from '@tiptap/react';
import StarterKit from '@tiptap/starter-kit';
import { PDFDownloadLink } from '@react-pdf/renderer';
import TiptapPDFDocument from './TiptapPDFDocument';

const MyEditor: React.FC = () => {
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

Would you like me to add support for bullet lists, code blocks, or images to this TSX version?
