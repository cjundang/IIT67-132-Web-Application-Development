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


# JSON & Web API เบื้องต้น

### พร้อมสาธิต: Process JSON บนเว็บด้วย `fetch`

ผู้เรียน: ปี 1 (ผ่าน HTML/CSS/Bootstrap/JS แล้ว)
ระยะเวลา: **2 ชั่วโมง** (บรรยาย + โค้ดจริง)

---

## เป้าหมายการเรียนรู้

* เข้าใจ **โครงสร้าง JSON** และกฎไวยากรณ์
* เรียนรู้แนวคิด **Web API / REST** และ HTTP เบื้องต้น
* สามารถใช้ **`fetch` + `async/await`** เพื่อ **GET/POST** ข้อมูล
* **Process JSON**: map/filter/reduce, destructuring, optional chaining
* แสดงผลข้อมูลในหน้าเว็บ (Bootstrap cards/table)
* จัดการ **error/loading** และพื้นฐาน **CORS**

> ผลลัพธ์สุดท้าย: ทำ mini app ดึงรายการจาก API + ค้นหา/กรอง + ส่งฟอร์ม (mock POST)

---

## โครงสร้างคลาส (2 ชั่วโมง)

1. (0:00–0:15) JSON คืออะไร + ไวยากรณ์
2. (0:15–0:30) Web API/HTTP/CORS พื้นฐาน
3. (0:30–1:15) สาธิต `fetch` GET → render → search/filter
4. (1:15–1:40) สาธิต POST/ส่งฟอร์ม + จัดการ error/loading
5. (1:40–2:00) Workshop & Q/A + สรุป/โจทย์บ้าน

---

# Part 1 — JSON คืออะไร?

* **JSON (JavaScript Object Notation)**: รูปแบบข้อมูลด้วยคู่ `key: value`
* ใช้ส่งข้อมูลระหว่าง client ↔ server
* ชนิดข้อมูลที่รองรับ: string, number, boolean, null, object `{}`, array `[]`

```json
{
  "id": 101,
  "title": "Intro to JSON",
  "tags": ["web", "api"],
  "author": { "name": "Ada", "active": true },
  "rating": 4.5,
  "published": null
}
```

**กฎสำคัญ**: key ต้องเป็น string (ใส่เครื่องหมายคำพูด), ไม่มีคอมเมนต์, ห้ามมี `undefined`/ฟังก์ชัน

---

## JSON vs JavaScript Object

* JSON = **string** ที่มีรูปแบบเฉพาะ
* JS Object = โครงสร้างข้อมูลในหน่วยความจำ

แปลงไปมา:

```js
const obj = { a: 1 };
const json = JSON.stringify(obj); // "{"a":1}"
const back = JSON.parse(json);    // { a: 1 }
```

ข้อควรระวัง: `JSON.parse` ต้องครอบด้วย `try/catch` เมื่อไม่แน่ใจว่าข้อมูลถูกต้อง

---

# Part 2 — Web API / HTTP เบื้องต้น

* **Endpoint**: URL ของทรัพยากร เช่น `/api/posts`
* **Method**: GET (อ่าน), POST (สร้าง), PUT/PATCH (แก้ไข), DELETE (ลบ)
* **Status Code**: 200 OK, 201 Created, 400 Bad Request, 401/403, 404, 500
* **Headers**: `Content-Type: application/json`, `Authorization: Bearer ...`
* **CORS**: นโยบาย cross-origin ของบราวเซอร์ ต้องมี header ฝั่ง server (เช่น `Access-Control-Allow-Origin`)

> สำหรับสาธิตจะใช้ API สาธารณะ/ mock ที่เปิด CORS เช่น **JSONPlaceholder** และ **ReqRes**

---

## โครงสร้างคำขอ (Request) & คำตอบ (Response)

**Request**

```
GET https://jsonplaceholder.typicode.com/posts?_limit=5
Accept: application/json
```

**Response (ย่อ)**

```json
[
  { "id": 1, "title": "...", "body": "..." },
  { "id": 2, "title": "...", "body": "..." }
]
```

---

# Part 3 — เริ่มใช้ `fetch`

**GET แบบง่าย**

```js
async function loadPosts() {
  const res = await fetch('https://jsonplaceholder.typicode.com/posts?_limit=5');
  if (!res.ok) throw new Error('HTTP ' + res.status);
  const data = await res.json(); // parse JSON → JS object/array
  console.log(data);
}
loadPosts().catch(console.error);
```

**แนวทางดีๆ**: ใช้ `try/catch`, ตรวจ `res.ok`, แยกฟังก์ชัน fetch ออกเป็น **service**

---

## โครงสร้างโปรเจคเล็ก (แนะนำ)

```
project/
  index.html
  styles.css         // (ถ้าใช้ Bootstrap CDN ก็ได้)
  js/
    api.js          // ฟังก์ชันเรียก API รวมกันไว้
    ui.js           // ฟังก์ชัน render DOM
    utils.js        // ชุด helper (format, debounce, paginate)
  assets/
```

---

# สาธิต 1: Render รายการเป็นการ์ด

**HTML (Bootstrap)**

```html
<link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/css/bootstrap.min.css">
<div class="container py-4">
  <h1 class="mb-3">Posts</h1>
  <input id="q" class="form-control mb-3" placeholder="ค้นหาหัวข้อ..." />
  <div id="grid" class="row g-3"></div>
</div>
```

**JS**

```js
// js/api.js
export async function getPosts() {
  const res = await fetch('https://jsonplaceholder.typicode.com/posts');
  if (!res.ok) throw new Error('HTTP ' + res.status);
  return res.json();
}

// js/ui.js
export function renderCards(container, items) {
  container.innerHTML = items.map(p => `
    <div class="col-12 col-md-6 col-lg-4">
      <div class="card h-100">
        <div class="card-body">
          <h5 class="card-title">${escapeHtml(p.title)}</h5>
          <p class="card-text">${escapeHtml(p.body).slice(0,120)}...</p>
        </div>
      </div>
    </div>`).join('');
}

// js/utils.js
export const escapeHtml = (s='') => s
  .replaceAll('&','&amp;')
  .replaceAll('<','&lt;')
  .replaceAll('>','&gt;');
```

**main.js**

```js
import { getPosts } from './js/api.js';
import { renderCards } from './js/ui.js';
import { escapeHtml } from './js/utils.js';

(async () => {
  try {
    const all = await getPosts();
    state.items = all;
    renderCards(document.querySelector('#grid'), all);
  } catch (err) {
    console.error(err);
  }
})();
```

---

# Process JSON: ค้นหา/กรอง/เรียง

```js
const qInput = document.querySelector('#q');
qInput.addEventListener('input', e => {
  const q = e.target.value.toLowerCase().trim();
  const filtered = state.items
    .filter(p => p.title.toLowerCase().includes(q) || p.body.toLowerCase().includes(q))
    .slice(0, 30) // limit เพื่อไม่ให้ช้าเกิน
  renderCards(document.querySelector('#grid'), filtered);
});
```

**เทคนิคประจำวัน**

* `map / filter / reduce`
* **Destructuring**: `const { id, title } = post`
* **Optional Chaining**: `user?.address?.city`
* **Nullish Coalescing**: `value ?? 'N/A'`

---
