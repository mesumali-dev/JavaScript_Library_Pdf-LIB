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

#### PDF-Lib Basics / Introduction
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
