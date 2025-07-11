To generate a **PDF with a hardcoded title page**, followed by dynamic **Tiptap editor content**, while also displaying **page numbers on every page**, follow this pattern:

---

## ✅ Overview

* Page 1: Hardcoded title
* Page 2+: Rendered from `editor.getJSON()` (Tiptap content)
* Page numbers: shown on **every page** using `fixed` and `render` in `<Text>`

---

## ✅ `TiptapPDFDocument.tsx` (with editor content + title page)

```tsx
import React from 'react';
import {
  Document,
  Page,
  Text,
  View,
  StyleSheet,
} from '@react-pdf/renderer';
import type { JSONContent } from '@tiptap/core';

const styles = StyleSheet.create({
  page: {
    padding: 60,
    fontSize: 12,
    fontFamily: 'Helvetica',
  },
  titlePage: {
    justifyContent: 'center',
    alignItems: 'center',
    textAlign: 'center',
  },
  title: {
    fontSize: 32,
    fontWeight: 'bold',
    marginBottom: 10,
  },
  subtitle: {
    fontSize: 18,
    marginBottom: 40,
  },
  paragraph: {
    marginBottom: 12,
    textAlign: 'left',
  },
  heading: {
    fontSize: 18,
    fontWeight: 'bold',
    marginBottom: 10,
    textAlign: 'left',
  },
  pageNumber: {
    position: 'absolute',
    bottom: 30,
    left: 0,
    right: 0,
    textAlign: 'center',
    fontSize: 10,
    color: 'grey',
  },
});

const renderTextWithMarks = (node: any, key: number) => {
  if (!node.text) return null;

  let textStyle: any = {};
  node.marks?.forEach((mark: any) => {
    if (mark.type === 'bold') textStyle.fontWeight = 'bold';
    if (mark.type === 'italic') textStyle.fontStyle = 'italic';
  });

  return (
    <Text key={key} style={textStyle}>
      {node.text}
    </Text>
  );
};

const renderContentNode = (node: any, idx: number) => {
  if (!node || !node.type) return null;

  if (node.type === 'heading') {
    return (
      <Text key={idx} style={styles.heading}>
        {node.content?.map((child: any, i: number) => renderTextWithMarks(child, i))}
      </Text>
    );
  }

  if (node.type === 'paragraph') {
    return (
      <Text key={idx} style={styles.paragraph}>
        {node.content?.map((child: any, i: number) => renderTextWithMarks(child, i))}
      </Text>
    );
  }

  return null; // Extend for other node types
};

const TiptapPDFDocument: React.FC<{ content: JSONContent }> = ({ content }) => (
  <Document>
    {/* Page 1: Hardcoded title */}
    <Page size="A4" style={[styles.page, styles.titlePage]}>
      <Text style={styles.title}>Gecko Security Report</Text>
      <Text style={styles.subtitle}>AI-powered vulnerability scan</Text>
      <Text>July 2025</Text>

      <Text
        style={styles.pageNumber}
        fixed
        render={({ pageNumber, totalPages }) => `${pageNumber} / ${totalPages}`}
      />
    </Page>

    {/* Page 2+: Tiptap Content */}
    <Page size="A4" style={styles.page}>
      {content?.content?.map((node, idx) => renderContentNode(node, idx))}

      <Text
        style={styles.pageNumber}
        fixed
        render={({ pageNumber, totalPages }) => `${pageNumber} / ${totalPages}`}
      />
    </Page>
  </Document>
);

export default TiptapPDFDocument;
```

---

### ✅ Usage in your editor component

```tsx
<PDFDownloadLink
  document={<TiptapPDFDocument content={editor.getJSON()} />}
  fileName="gecko-report.pdf"
>
  {({ loading }) => (
    <button disabled={loading}>
      {loading ? 'Generating PDF...' : 'Download PDF'}
    </button>
  )}
</PDFDownloadLink>
```

---

### ✅ What This Gives You

* Title page → hardcoded
* Page 2+ → rendered from `Tiptap` JSON
* Page numbers → shown on **every page**, including the title page
* Extendable for list items, code blocks, images, etc.

---

Would you like to paginate the Tiptap content across multiple pages if it’s long?
