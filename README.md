# PDF-Lib Learning Roadmap
**From Beginner → Advanced**

---

## 1. Basics / Introduction
- Pdf-lib kya hai aur kyun use hota hai
- Project setup: Next.js + pdf-lib
- Basic PDF create karna
- PDF save/export karna

---

## 2. Text Manipulation
- Simple text add karna
- Font set karna (Standard fonts, Custom fonts)
- Font size, color aur style change karna
- Multiple lines aur paragraph handle karna

---

## 3. Images
- Image add karna (PNG / JPG)
- Image size aur position adjust karna
- Image transparency aur rotation

---

## 4. Pages
- Multiple pages create karna
- Existing PDF open aur edit karna
- Page size aur layout set karna

---

## 5. Shapes & Drawing
- Rectangle, line, circle draw karna
- Color fill aur stroke set karna
- Position aur rotation control

---

## 6. Tables / Structured Layouts
- Simple table create karna (rows & columns)
- Nested columns / merged cells
- Dynamic data se table fill karna
- Borders aur styling

---

## 7. Advanced Features
- PDF Metadata (title, author, subject) set karna
- Links aur annotations add karna
- Forms / fields create karna (text fields, checkboxes)
- Encryption / password protect

---

## 8. Optimization & Best Practices
- PDF size reduce karna
- Reusable templates / components
- Async handling & performance in Next.js

---

# PDF-Lib Basics / Introduction

## 1.1 Pdf-lib kya hai aur kyun use hota hai

pdf-lib ek JavaScript library hai jo aapko PDF create aur edit karne ka option deti hai.  

Iska use mostly web apps (Next.js, React, Node.js) mein hota hai.

**Features:**

- PDF create karna from scratch
- Existing PDF edit karna
- Text, images, shapes, tables, forms add karna
- Download / save karna browser aur server dono mein

**Example use cases:**

- Invoice generate karna
- Reports create karna
- Certificates / tickets generate karna

## 1.2 Project Setup (Next.js + pdf-lib)

Next.js project create karo (agar already nahi hai):
```bash
npx create-next-app my-pdf-app  
cd my-pdf-app
```

Pdf-lib install karo:
```bash
npm install pdf-lib
```

## 1.3 Basic PDF Create Karna

`pages/index.js` mein simple PDF create karte hain:
```typescript
import { PDFDocument, rgb } from 'pdf-lib';

export default function Home() {  
  const createPdf = async () => {  
    // 1. New PDF document create karo  
    const pdfDoc = await PDFDocument.create();  

    // 2. Ek new page add karo  
    const page = pdfDoc.addPage([600, 400]); // width:600, height:400  

    // 3. Text add karo  
    page.drawText('Hello PDF-Lib!', {  
      x: 50,  
      y: 350,  
      size: 30,  
      color: rgb(0, 0.53, 0.71),  
    });  

    // 4. PDF ko byte array mein convert karo  
    const pdfBytes = await pdfDoc.save();  

    // 5. Browser mein download karo  
    const blob = new Blob([pdfBytes], { type: 'application/pdf' });  
    const link = document.createElement('a');  
    link.href = URL.createObjectURL(blob);  
    link.download = 'example.pdf';  
    link.click();  
  };  

  return (  
    <div style={{ padding: '50px' }}>  
      <button onClick={createPdf}>Create PDF</button>  
    </div>  
  );  
}
```
**Explanation:**

- `PDFDocument.create()` → new PDF banata hai  
- `addPage([width, height])` → page add karta hai  
- `drawText()` → page pe text add karta hai  
- `pdfDoc.save()` → PDF bytes generate karta hai  
- Browser mein download ke liye blob aur link use kiya

## 1.4 PDF Save / Export Karna

- Above example mein browser download method dikhaya.  
- Server-side Next.js (API route) mein bhi PDF generate karke return kar sakte ho:
```typescript
import { PDFDocument, rgb } from 'pdf-lib';  

export default async function handler(req, res) {  
  const pdfDoc = await PDFDocument.create();  
  const page = pdfDoc.addPage([600, 400]);  
  page.drawText('Server-side PDF!', { x: 50, y: 350, size: 30, color: rgb(1, 0, 0) });  
  const pdfBytes = await pdfDoc.save();  

  res.setHeader('Content-Type', 'application/pdf');  
  res.setHeader('Content-Disposition', 'attachment; filename=server.pdf');  
  res.send(Buffer.from(pdfBytes));  
}
```
- Browser mein API call karke bhi download karwa sakte ho.

✅ **Summary / Key Points:**

- Pdf-lib se simple PDF create karna easy hai  
- Browser aur server dono mein use hota hai  
- Text, pages aur download initial step hai

---


# 2. Text Manipulation in pdf-lib

## 2.1 Simple Text Add Karna

Already basics mein humne drawText use kiya tha:

page.drawText('Hello PDF-Lib!', { x: 50, y: 350 });

- `x` aur `y` → position (bottom-left corner se)  
- size → font size (default 12)  
- `color` → rgb color `{r,g,b}` 0–1 scale

## 2.2 Font Set Karna

**Standard fonts** available hain pdf-lib mein:
```typescript
import { StandardFonts } from 'pdf-lib';

const timesRomanFont = await pdfDoc.embedFont(StandardFonts.TimesRoman);  
page.drawText('Custom Font Text', { x: 50, y: 300, font: timesRomanFont, size: 24 });
```
**Available Standard Fonts:**

- TimesRoman  
- Helvetica  
- Courier  
- Symbol  
- ZapfDingbats  

Custom Fonts bhi embed kar sakte ho (TTF/OTF):

import fs from 'fs';  

const fontBytes = fs.readFileSync('./public/fonts/YourFont.ttf');  
const customFont = await pdfDoc.embedFont(fontBytes);  
page.drawText('Custom Font!', { x: 50, y: 250, font: customFont, size: 24 });

## 2.3 Font Size, Color, Style

page.drawText('Big Red Text', { x: 50, y: 200, size: 36, color: rgb(1, 0, 0) });  
page.drawText('Small Blue Text', { x: 50, y: 150, size: 12, color: rgb(0, 0, 1) });

- Bold / Italic → standard fonts se limited, custom font use kar ke achieve hota hai

## 2.4 Multiple Lines / Paragraph

Pdf-lib automatically line break nahi karta. Aapko manually y coordinate adjust karna hoga:

const lines = ['This is line 1', 'This is line 2', 'This is line 3'];  
let y = 400;  

lines.forEach(line => {  
  page.drawText(line, { x: 50, y, size: 20, color: rgb(0,0,0) });  
  y -= 25; // line spacing  
});

- y -= 25 → line height / spacing adjust karta hai  
- Pro Tip: Loop ke through dynamic content (e.g., invoice items) easily add kar sakte ho

✅ **Summary / Key Points:**

- drawText + position + size + color → basic text  
- Fonts: standard & custom  
- Multiple lines manually handle hoti hain
