## PDF-Lib Learning Roadmap (Beginner → Advanced)

#### 1. Basics / Introduction

1. Pdf-lib kya hai aur kyun use hota hai
2. Project setup (Next.js + pdf-lib)
3. Basic PDF create karna
4. PDF save/export karna

#### 2. Text Manipulation

1. Simple text add karna
2. Font set karna (Standard fonts, Custom fonts)
3. Font size, color aur style change karna
4. Multiple lines aur paragraph handle karna

#### 3. Images

1. Image add karna (PNG/JPG)
2. Image size aur position adjust karna
3. Image transparency / rotation

#### 4. Pages

1. Multiple pages create karna
2. Existing PDF open aur edit karna
3. Page size aur layout set karna

#### 5. Shapes & Drawing

1. Rectangle, line, circle draw karna
2. Color fill aur stroke set karna
3. Position aur rotation control

#### 6. Tables / Structured Layouts

1. Simple table create karna (rows & columns)
2. Nested columns / merged cells
3. Dynamic data se table fill karna
4. Borders aur styling

#### 7. Advanced Features

1. PDF Metadata (title, author, subject) set karna
2. Links aur annotations add karna
3. Forms / fields create karna (text fields, checkboxes)
4. Encryption / password protect

#### 8. Optimization & Best Practices

1. PDF size reduce karna
2. Reusable templates / components
3. Async handling & performance in Next.js

---

#### 1. PDF-Lib Basics / Introduction
#### 1.1 Pdf-lib kya hai aur kyun use hota hai

- pdf-lib ek JavaScript library hai jo aapko PDF create aur edit karne ka option deti hai.
- Iska use mostly web apps (Next.js, React, Node.js) mein hota hai.
- Features:
    - PDF create karna from scratch
    - Existing PDF edit karna
    - Text, images, shapes, tables, forms add karna
    - Download / save karna browser aur server dono mein

##### Example use cases:

- Invoice generate karna
- Reports create karna
- Certificates / tickets generate karna

#### 1.2 Project Setup (Next.js + pdf-lib)

1. Next.js project create karo (agar already nahi hai):
```typescript
npx create-next-app my-pdf-app
cd my-pdf-app
```

2. Pdf-lib install karo:
```typescript
npm install pdf-lib
```

#### 1.3 Basic PDF Create Karna

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

##### Explanation:

- `PDFDocument.create()` → new PDF banata hai
- `addPage([width, height])` → page add karta hai
- `drawText()` → page pe text add karta hai
- `pdfDoc.save()` → PDF bytes generate karta hai
- Browser mein download ke liye blob aur link use kiya

#### 1.4 PDF Save / Export Karna

- Above example mein browser download method dikhaya
- Server-side Next.js (API route) mein bhi PDF generate karke return kar sakte ho:
```typescript
// pages/api/create-pdf.js
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
- Browser mein API call karke bhi download karwa sakte ho

#### Summary / Key Points:

- Pdf-lib se simple PDF create karna easy hai
- Browser aur server dono mein use hota hai
- Text, pages aur download initial step hai

--- 

#### 2. Text Manipulation in pdf-lib
#### 2.1 Simple Text Add Karna

Already basics mein humne drawText use kiya tha:
```typescript
page.drawText('Hello PDF-Lib!', { x: 50, y: 350 });
```
- `x` aur `y` → position (bottom-left corner se)
- `size` → font size (default 12)
- `color` → rgb color `{r,g,b}` 0–1 scale

#### 2.2 Font Set Karna

**Standard fonts** available hain pdf-lib mein:
```typescript
import { StandardFonts } from 'pdf-lib';

const timesRomanFont = await pdfDoc.embedFont(StandardFonts.TimesRoman);
page.drawText('Custom Font Text', { x: 50, y: 300, font: timesRomanFont, size: 24 });
```
- **Available Standard Fonts:**
  
    - TimesRoman
    - Helvetica
    - Courier
    - Symbol
    - ZapfDingbats
  
**Custom Fonts** bhi embed kar sakte ho (TTF/OTF):
```typescript
import fs from 'fs';

const fontBytes = fs.readFileSync('./public/fonts/YourFont.ttf');
const customFont = await pdfDoc.embedFont(fontBytes);
page.drawText('Custom Font!', { x: 50, y: 250, font: customFont, size: 24 });
```

#### 2.3 Font Size, Color, Style

```typescript
page.drawText('Big Red Text', {
  x: 50,
  y: 200,
  size: 36,
  color: rgb(1, 0, 0), // Red
});

page.drawText('Small Blue Text', {
  x: 50,
  y: 150,
  size: 12,
  color: rgb(0, 0, 1), // Blue
});
```
- **Bold / Italic** → standard fonts se limited, custom font use kar ke achieve hota hai

#### 2.4 Multiple Lines / Paragraph

Pdf-lib automatically line break nahi karta. Aapko manually `y` coordinate adjust karna hoga:
