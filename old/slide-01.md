---
marp: true
theme: default
size: 16:9
paginate: true
title: React Navigation and Lesson App
description: สไลด์สรุปจากเอกสาร 07_th.md
backgroundImage: url('wallpaper.png')
backgroundSize: cover
style: |
  section {
    color: blue; /* สีตัวอักษรทั้งหมด */
  }
  h1 {
    color: black;
    font-size: 56px !important;
    line-height: 1.1;
    margin: 0.2em 0 0.4em;
    
  }
  h2 {
    color: black;
    font-size: 56px !important;
    line-height: 1.1;
    margin: 0.2em 0 0.4em;
    
  }
  h3 {
    color: black;
  }
---

# Week 1: HTML พื้นฐาน
## โฟกัสคาบนี้
- สอน–สาธิตทีละ **แท็ก/คำสั่ง** ตามเอกสารประกอบ
- สร้างหน้า HTML5 ที่ถูกต้อง + โครงสร้างเชิงความหมาย (semantic)

---

## Roadmap ของคาบ
1) โครงเอกสาร (Document Structure)  
2) เนื้อหาข้อความและหัวเรื่อง (Text & Headings)  
3) รายการ (Lists)  
4) ลิงก์และสื่อ (Links & Media)  
5) โครงหน้าเชิงความหมาย (Semantic Sectioning)

---

## 1) Document Structure — ภาพรวม
องค์ประกอบมาตรฐานของเอกสาร HTML5:
`<!DOCTYPE html>`, `<html lang>`, `<head>`, `<meta ...>`, `<title>`, `<body>`

**เป้าหมาย:** นักศึกษาสามารถระบุและอธิบายหน้าที่ของแต่ละแท็ก และเริ่มหน้า HTML ที่ถูกต้อง

---

## 1.1 `<!DOCTYPE html>`
**หน้าที่:** บอกเบราว์เซอร์ว่าเอกสารใช้มาตรฐาน HTML5

**สาธิต:**
```html
<!DOCTYPE html>
<html lang="th">
<head>
  <meta charset="utf-8">
  <title>เอกสารตัวอย่าง</title>
</head>
<body>สวัสดี HTML5</body>
</html>
````

**Mini-lab:** สร้างไฟล์ใหม่ `index.html` ใส่ `<!DOCTYPE html>` แล้วเปิดในเบราว์เซอร์

---

## 1.2 `<html lang="th">`

**หน้าที่:** โหนดราก + ระบุภาษาหลักเพื่อการเข้าถึง (a11y) และ SEO

**สาธิต/ตรวจสอบ:**

* เปลี่ยน `lang="th"` เป็น `"en"` เพื่อดูผลต่อเครื่องมืออ่านหน้าจอ (อธิบายเชิงแนวคิด)

**Mini-lab:** ตั้งค่า `lang` ให้ตรงกับภาษาของเนื้อหาในหน้า

---

## 1.3 `<head>` และเมตาเบื้องต้น

**หน้าที่:** กำหนดข้อมูลเมตา/การตั้งค่าหน้า

**สาธิต:**

```html
<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <title>โปรไฟล์ส่วนตัว</title>
</head>
```

**อธิบาย:**

* `charset="utf-8"` เข้ารหัสอักขระสากล
* `viewport` ให้แสดงผลเหมาะกับมือถือ


---

## 1.4 `<title>`

**หน้าที่:** ชื่อหน้าในแท็บเบราว์เซอร์ / ใช้โดยเครื่องมือค้นหา

**สาธิต:** เปลี่ยนข้อความ `<title>` แล้วรีเฟรช ดูผลที่แท็บ

**Mini-lab:** กำหนดชื่อหน้าให้สื่อความหมายและกระชับ

---

## 1.5 `<body>`

**หน้าที่:** เนื้อหาที่ผู้ใช้เห็น

**สาธิต:**

```html
<body>
  สวัสดี!
</body>
```

**Mini-lab:** ใส่ย่อหน้าแรกของหน้าโปรไฟล์ใน `<body>`

---

## 2) Text & Headings — ภาพรวม

แท็กแกน: `h1–h6`, `p`

**หลักการ:**

* มีเพียง **หนึ่ง `h1`** ต่อหน้า
* หัวเรื่องลดหลั่นระดับ (`h2` ใต้ `h1` ฯลฯ)
* ข้อความทั่วไปใช้ `p`

---

## 2.1 `h1–h6`

**สาธิต:**

```html
<main>
  <h1>โปรไฟล์ของฉัน</h1>
  <h2>เกี่ยวกับฉัน</h2>
  <h3>การศึกษา</h3>
</main>
```

**ข้อควรระวัง:** อย่ากระโดดจาก `h1` ไป `h4` โดยไร้เหตุผล

**Mini-lab:** จัดลำดับหัวเรื่องในหน้าโปรไฟล์ให้ถูกต้อง

---

## 2.2 `p`

**สาธิต:**

```html
<section>
  <h2>สรุปย่อ</h2>
  <p>ฉันเป็นผู้เรียนที่สนใจพัฒนาเว็บและการออกแบบส่วนติดต่อผู้ใช้</p>
  <p>กำลังฝึกทักษะ HTML/CSS/JS</p>
</section>
```

**Mini-lab:** เขียน 2 ย่อหน้าแนะนำตนเอง

---

## 3) Lists — ภาพรวม

แท็ก: `ul`, `ol`, `li`

**ใช้งาน:** แสดงรายการทักษะ/ประสบการณ์/ลำดับขั้นตอน

---

## 3.1 `ul` / 3.2 `ol` + `li`

**สาธิต:**

```html
<h2>ทักษะ</h2>
<ul>
  <li>HTML พื้นฐาน</li>
  <li>โครงสร้างเอกสาร</li>
</ul>

<h2>เป้าหมายการเรียน</h2>
<ol>
  <li>สร้างหน้าเว็บอย่างถูกต้อง</li>
  <li>ใช้ semantic tags</li>
</ol>
```

**Mini-lab:** เพิ่ม `ul` สำหรับทักษะ และ `ol` สำหรับเป้าหมาย

---

## 4) Links & Media — ภาพรวม

แท็ก: `a`, `img`

**หลักการ:**

* `a` ใช้ลิงก์ภายใน/ภายนอก/แองเคอร์
* `img` ต้องมี `alt` อธิบายความหมายของรูป

---

## 4.1 `a` (ลิงก์)

**สาธิต:**

```html
<nav>
  <a href="#about">เกี่ยวกับฉัน</a> |
  <a href="https://developer.mozilla.org/" target="_blank" rel="noopener">MDN</a>
</nav>
```

* Anchor ภายในหน้า: `href="#about"`
* ลิงก์ภายนอก: เพิ่ม `target="_blank"` และ `rel="noopener"`

**Mini-lab:** เพิ่มเมนูไปยังหัวข้อหลักในหน้า

---

## 4.2 `img` (รูปภาพ)

**สาธิต:**

```html
<img src="images/me.jpg" alt="รูปถ่ายผู้เรียนยืนหน้าบอร์ดผลงาน" width="240">
```

**แนวปฏิบัติ:** `alt` บรรยาย “ความหมาย” ของรูป ไม่ใช่คำตกแต่ง

**Mini-lab:** ใส่รูปโปรไฟล์ + เขียน `alt` ให้สื่อความหมาย

---

## 5) Semantic Sectioning — ภาพรวม

แท็ก: `header`, `nav`, `main`, `section`, `article`, `aside`, `footer`

**วัตถุประสงค์:** โครงหน้าอ่านง่าย เข้าถึงได้ดี และดูแลรักษาง่าย

---



## 5.1 โครงหน้าแบบ semantic — (Header & Nav)

```html
<body>
  <header>
    <h1>ชื่อ–สกุล</h1>
    <nav>
      <a href="#about">เกี่ยวกับฉัน</a> | <a href="#projects">ผลงาน</a>
    </nav>
  </header>
````

**หลักการ:**

* ใช้ `header` สำหรับส่วนหัวของหน้า/ส่วนย่อย
* `nav` รวมลิงก์นำทางหลักของเอกสาร
* ควรมีเพียงหนึ่ง `h1` ต่อหน้า (ระบุหัวเรื่องหลัก)

**Mini-lab:**

* แทน `div` ที่ครอบเมนูเดิมด้วย `nav` / ตั้งชื่อ `h1` ให้สื่อวัตถุประสงค์ของหน้าอย่างย่อ


---


## 5.1 โครงหน้าแบบ semantic — (Main & Sections)

```html
  <main id="about">
    <section>
      <h2>เกี่ยวกับฉัน</h2>
      <p>ย่อหน้าแนะนำตัว</p>
    </section>

    <section id="projects">
      <h2>ผลงานเด่น</h2>
      <article>
        <h3>โครงการ A</h3>
        <p>คำอธิบายสั้น ๆ</p>
      </article>
    </section>
  </main>
````

---
## 5.1 โครงหน้าแบบ semantic — (Main & Sections)

**หลักการ:**

* `main` คือเนื้อหาหลักของหน้า (ควรมีเพียงหนึ่งเดียว)
* `section` ใช้จัดหมวดสาระที่มีหัวข้อร่วม (`h2` เป็นต้น)
* `article` ใช้กับหน่วยเนื้อหาที่เป็นอิสระ (การ์ดผลงาน/โพสต์)

**Mini-lab:**

* แยก “เกี่ยวกับฉัน” และ “ผลงาน” เป็นสอง `section`
* นำการ์ดผลงานอย่างน้อย 1 รายการไปอยู่ใน `article`


---
## 5.1 โครงหน้าแบบ semantic — (Aside & Footer)

```html
  <aside>
    <h2>ทักษะ</h2>
    <ul><li>HTML</li><li>โครงสร้าง</li></ul>
  </aside>

  <footer id="contact">
    <p>อีเมล: you@example.com</p>
  </footer>
</body>
````
---
## 5.1 โครงหน้าแบบ semantic — (Aside & Footer)


**หลักการ:**

* `aside` รองรับเนื้อหาประกอบ/เสริม (เช่น แถบข้าง รายการทักษะ)
* `footer` สำหรับข้อมูลท้ายหน้า เช่น ช่องทางติดต่อ/ลิขสิทธิ์

**Mini-lab:**

* ย้าย “ทักษะ/ลิงก์ที่เกี่ยวข้อง” ไปไว้ใน `aside`
* สร้าง `footer` พร้อมข้อมูลติดต่อ (อีเมล/ลิงก์โซเชียล) และ anchor `id="contact"`



---
## 5.1 โครงหน้าแบบ semantic — (Aside & Footer)
### Common Pitfalls & Checks

* ลืม `<!DOCTYPE html>` / `lang` ไม่ตรงภาษา
* ข้ามระดับหัวเรื่อง (`h1` ➜ `h4`)
* `img` ไม่มี `alt`
* ไม่ใส่เครื่องหมายคำพูดรอบค่าของแอตทริบิวต์
  **Quick Check:** เปิด DevTools ➜ ตรวจลำดับหัวเรื่องและโครงสร้าง sectioning

---

## งานสรุปคาบ (Checkpoint)

1. โครง `head` ครบ (`charset`, `viewport`, `title`)
2. โครงหน้า semantic ขั้นต่ำ: `header`, `nav`, `main`, `section/article`, `footer`
3. เมนู `a` ภายใน/ภายนอก + `img` พร้อม `alt`
   ส่งไฟล์ `index.html` + สกรีนช็อตผลลัพธ์


