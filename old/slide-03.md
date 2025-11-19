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

# Bootstrap เบื้องต้น

**ติดตั้งผ่าน CDN · Grid System · Utilities · Components (Navbar, Card, Button, Modal)**

ผู้เรียน: นศ. ปี 1 (ผ่าน HTML + CSS มาแล้ว)

⏱ ระยะเวลา: 2 ชม. (บรรยาย + สาธิต + ลงมือทำ)

---

## วัตถุประสงค์การเรียนรู้ (Learning Objectives)

เมื่อจบคลาสนี้ คุณจะสามารถ…

1. ติดตั้ง Bootstrap ผ่าน **CDN** และใช้ **Starter Template** ได้
2. อธิบายแนวคิด **Container → Row → Column** ของ **Grid System** ได้
3. ใช้ **Utilities** ยอดนิยม (spacing, color, display, flex, typography) ได้
4. ประกอบหน้าเว็บด้วย **Components** พื้นฐาน: Navbar, Card, Button, Modal
5. สร้างหน้าเว็บ **responsive** อย่างรวดเร็ว

---

## โครงสร้างคลาส (2 ชม.)

* 0:00–0:10 บทนำ + ทำไมต้อง Bootstrap
* 0:10–0:25 ติดตั้งผ่าน CDN + Starter Template
* 0:25–0:55 Grid System (container, row, col, breakpoint)
* 0:55–1:20 Utilities ที่ใช้บ่อย
* 1:20–1:45 Components: Navbar, Card, Button, Modal (สาธิต)
* 1:45–2:00 Mini-lab: Homepage แบบ Responsive + Q&A

---

## ทำไมต้อง Bootstrap?

* ช่วยทำ **responsive** ได้ไว ไม่ต้องเขียน CSS เองทั้งหมด
* มี **ระบบ Grid** และ **Components** ให้ใช้งานทันที
* มี **Utilities** สำเร็จรูป ลดเวลาจัด spacing/สี/typography
* เอกสารครบถ้วน ตัวอย่างเยอะ

> เหมาะกับผู้เริ่มต้นที่อยากเห็นผลลัพธ์เร็ว และค่อย ๆ ศึกษา CSS เพิ่มเติม

---

## ติดตั้ง Bootstrap ผ่าน CDN

วางใน `<head>` และก่อนปิด `</body>`

```html
<!-- CSS -->
<link
  href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/css/bootstrap.min.css"
  rel="stylesheet"
  integrity="sha384-QWTKZyjpPEjISv5WaRU9OFeRpok6YctnYmDr5pNlyT2bRjXh0JMhjY6hW+ALEwIH"
  crossorigin="anonymous"
/>

<!-- JS (Bundle รวม Popper) -->
<script
  src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/js/bootstrap.bundle.min.js"
  integrity="sha384-YvpcrYf0tY3lHB60NNkmXc5s9fDVZLESaAA55NDzOxhy9GkcIdslK1eN7N6jIeHz"
  crossorigin="anonymous"
></script>
```

> ใช้ CDN เพื่อเริ่มเร็ว ไม่ต้องติดตั้งเครื่องมือเพิ่มเติม

---

## Starter Template (Copy & Go)

```html
<!doctype html>
<html lang="th">
  <head>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1" />
    <title>My Bootstrap Page</title>
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/css/bootstrap.min.css" rel="stylesheet" />
  </head>
  <body>
    <h1 class="text-center p-4">สวัสดี Bootstrap 👋</h1>

    <div class="container">
      <p class="lead">หน้าแรกที่สร้างด้วย Bootstrap</p>
    </div>

    <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/js/bootstrap.bundle.min.js"></script>
  </body>
</html>
```

---

## แนวคิด Grid System

* **Container**: กล่องหลักของหน้า (`.container`, `.container-fluid`)
* **Row**: แถว (`.row`) สำหรับจัดคอลัมน์
* **Column**: คอลัมน์ (`.col`, `.col-6`, `.col-sm-4`, …)
* แบ่ง 12 ส่วนต่อแถว
* **Breakpoint** (จุดเปลี่ยนหน้าจอ): `sm ≥576px`, `md ≥768px`, `lg ≥992px`, `xl ≥1200px`, `xxl ≥1400px`

---

## ตัวอย่าง Grid: คอลัมน์เท่ากัน

```html
<div class="container">
  <div class="row">
    <div class="col border p-3">A</div>
    <div class="col border p-3">B</div>
    <div class="col border p-3">C</div>
  </div>
</div>
```

* ใช้ `.col` แบบไม่กำหนดเลข → แบ่งพื้นที่เท่า ๆ กัน

---

## ตัวอย่าง Grid: คอลัมน์ตามสัดส่วน

```html
<div class="container">
  <div class="row">
    <div class="col-4 border p-3">25%</div>
    <div class="col-8 border p-3">75%</div>
  </div>
</div>
```

* ตัวเลขรวมกัน ≤ 12 ในหนึ่งแถว

---

## ตัวอย่าง Grid: Responsive ด้วย Breakpoints

```html
<div class="container">
  <div class="row">
    <div class="col-12 col-md-6 col-lg-4 p-3 bg-light">กล่อง 1</div>
    <div class="col-12 col-md-6 col-lg-4 p-3 bg-light">กล่อง 2</div>
    <div class="col-12 col-md-6 col-lg-4 p-3 bg-light">กล่อง 3</div>
  </div>
</div>
```

* มือถือ: เต็มแถว (12)
* แท็บเล็ต: 2 คอลัมน์ (6 + 6)
* เดสก์ท็อป: 3 คอลัมน์ (4 + 4 + 4)

---

## Utilities ที่ใช้บ่อย — Spacing

* `m-*` = margin, `p-*` = padding (0–5)
* เช่น `mt-3`, `mb-2`, `px-4`, `py-5`

```html
<div class="p-4 m-3 border rounded">ช่องว่างด้วย Utilities</div>
```

---

## Utilities — Colors & Background

* ข้อความ: `text-primary`, `text-success`, `text-danger`, `text-muted`
* พื้นหลัง: `bg-light`, `bg-dark`, `bg-warning`, `bg-info`, …

```html
<p class="text-success">ผ่านจ้า ✓</p>
<div class="bg-warning p-3">คำเตือน</div>
```

---

## Utilities — Display & Flex

* แสดง/ซ่อน: `d-none`, `d-block`, `d-md-flex`
* Flex: `d-flex`, `justify-content-between`, `align-items-center`, `gap-3`

```html
<div class="d-flex justify-content-between align-items-center p-3 border">
  <span>ซ้าย</span>
  <span>ขวา</span>
</div>
```

---

## Utilities — Typography & Images

* ตัวหนังสือ: `fw-bold`, `fst-italic`, `text-center`, `lead`
* รูปภาพ responsive: `img-fluid`

```html
<h2 class="fw-bold text-center">หัวข้อเด่น</h2>
<img src="https://picsum.photos/800/300" class="img-fluid rounded" alt="random" />
```

---

## Component: Navbar (พื้นฐาน)

```html
<nav class="navbar navbar-expand-lg bg-body-tertiary">
  <div class="container">
    <a class="navbar-brand" href="#">MySite</a>
    <button class="navbar-toggler" type="button" data-bs-toggle="collapse" data-bs-target="#navMain">
      <span class="navbar-toggler-icon"></span>
    </button>
    <div class="collapse navbar-collapse" id="navMain">
      <ul class="navbar-nav ms-auto">
        <li class="nav-item"><a class="nav-link" href="#">Home</a></li>
        <li class="nav-item"><a class="nav-link" href="#">Features</a></li>
        <li class="nav-item"><a class="nav-link" href="#">Contact</a></li>
      </ul>
    </div>
  </div>
</nav>
```

> `navbar-expand-lg` = ขยายเมนูเมื่อจอ ≥ lg, mobile จะเป็นปุ่ม hamburger

---

## Component: Card

```html
<div class="container my-4">
  <div class="row g-4">
    <div class="col-12 col-md-6 col-lg-4">
      <div class="card h-100">
        <img src="https://picsum.photos/600/400" class="card-img-top" alt="..." />
        <div class="card-body">
          <h5 class="card-title">Card title</h5>
          <p class="card-text">คำอธิบายสั้น ๆ ของเนื้อหาในการ์ด</p>
          <a href="#" class="btn btn-primary">ดูเพิ่มเติม</a>
        </div>
      </div>
    </div>
  </div>
</div>
```

---

## Component: Button

```html
<button class="btn btn-primary">Primary</button>
<button class="btn btn-outline-secondary">Outline</button>
<button class="btn btn-success btn-sm">Small</button>
```

* ขนาด: `btn-sm`, `btn-lg`
* สไตล์: `btn-primary`, `btn-secondary`, `btn-danger`, `btn-outline-*`

---

## Component: Modal

```html
<!-- ปุ่มเปิด Modal -->
<button class="btn btn-primary" data-bs-toggle="modal" data-bs-target="#myModal">
  เปิด Modal
</button>

<!-- โครง Modal -->
<div class="modal fade" id="myModal" tabindex="-1">
  <div class="modal-dialog">
    <div class="modal-content">
      <div class="modal-header">
        <h5 class="modal-title">หัวข้อ</h5>
        <button type="button" class="btn-close" data-bs-dismiss="modal"></button>
      </div>
      <div class="modal-body">
        สวัสดีจาก Modal!
      </div>
      <div class="modal-footer">
        <button class="btn btn-secondary" data-bs-dismiss="modal">ปิด</button>
        <button class="btn btn-primary">บันทึก</button>
      </div>
    </div>
  </div>
</div>
```

> ต้องมี **bootstrap.bundle.min.js** (มี Popper) เพื่อให้ Modal ทำงาน

---

## สาธิตสด (Live Demo)

1. ตั้งค่า Starter Template + ใส่ Navbar

2. สร้าง Grid 3 คอลัมน์ (มือถือ 1, แท็บเล็ต 2, เดสก์ท็อป 3)

3. ใส่ Card 3 ใบในกริด + ปุ่ม `ดูเพิ่มเติม`

4. เพิ่มปุ่ม `ติดต่อเรา` เปิด Modal แบบฟอร์มสั้น ๆ (ชื่อ + อีเมล)

---

## Mini-lab (15–20 นาที)

**โจทย์:**

* ทำหน้า Landing Page ร้านกาแฟเล็ก ๆ
* โครง: Navbar + Hero (รูปใหญ่ + ข้อความ) + เมนู (การ์ด 3 ใบ) + ปุ่มเปิด Modal “สั่งเลย”
* Responsive: มือถือ 1 คอลัมน์, md ≥ 2 คอลัมน์, lg ≥ 3 คอลัมน์
* ใช้ Utilities จัด spacing สี และจัดวางด้วย flex ที่ต้องการ

> เป้าหมาย: เห็นหน้าเสร็จในเวลาอันสั้นและเรียนรู้วิธีดัดแปลง

---

## โครงฟอร์มใน Modal (ตัวอย่างสำหรับ Lab)

```html
<form>
  <div class="mb-3">
    <label class="form-label">ชื่อ</label>
    <input type="text" class="form-control" />
  </div>
  <div class="mb-3">
    <label class="form-label">อีเมล</label>
    <input type="email" class="form-control" />
  </div>
  <button type="submit" class="btn btn-primary">ส่ง</button>
</form>
```

---

## เคล็ดลับ & ข้อควรระวัง

* ใช้ `.container` ครอบเนื้อหาเพื่อให้ระยะขอบสวยงาม
* ใช้คลาส **breakpoint** ให้ถูก (`col-12 col-md-6 col-lg-4`)
* อย่าลืมใส่ **viewport meta** ใน `<head>`
* ถ้า Modal/Dropdown ไม่ทำงาน → ตรวจสอบว่าใช้ **bundle JS** ถูกไฟล์
* ใช้ **Utilities ก่อน** ค่อยค่อยเขียน CSS เองเมื่อจำเป็น

---

## โครงสร้างไฟล์แนะนำ (สำหรับโปรเจ็กต์เล็ก)

```
project/
├─ index.html        # หน้าแรก (CDN Bootstrap)
├─ images/           # รูปภาพ
└─ styles.css        # (ถ้าต้องการปรับแต่งเพิ่มเล็กน้อย)
```

ใน `index.html` ใส่ `<link rel="stylesheet" href="styles.css">` ต่อจาก Bootstrap

---

## สรุป

* Bootstrap = ทางลัดทำเว็บ responsive
* เริ่มด้วย **CDN + Starter Template**
* จัดเลย์เอาต์ด้วย **Grid (12 ส่วน)**
* ปรับหน้าตาอย่างไวด้วย **Utilities**
* เติม **Components** ให้ครบหน้า (Navbar/Card/Button/Modal)

---

## งานหลังเรียน (Optional)

* แปลงหน้า Lab ให้เป็นธีมอื่น (เช่น เบเกอรี/เสื้อผ้า)
* เพิ่มส่วน **Gallery** ใช้กริดภาพ + `img-fluid`
* ลองเปลี่ยนสีปุ่ม/พื้นหลังด้วย Utilities

---

## Q&A

มีคำถามไหม? 😊

> แชร์หน้าที่ทำแล้ว เปิดโค้ดให้ดูร่วมกัน และแก้ไขสด
