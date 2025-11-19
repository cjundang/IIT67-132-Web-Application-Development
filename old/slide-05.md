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



## DOM & การจัดการ Event (Action)

* เลือก element:

```js
const btn = document.querySelector("#btn")
const input = document.getElementById("name")
```

* ผูก event:

```js
btn.addEventListener("click", () => {
  console.log("clicked")
})
```

* อ่าน/เขียนค่า:

```js
input.value = "Alice"
document.querySelector("#out").textContent = "Hello!"
```

---

## Prevent Default & Event Object

```html
<form id="login">
  <input name="user" required>
  <button type="submit">Login</button>
</form>
```

```js
document.getElementById("login").addEventListener("submit", (e) => {
  e.preventDefault() // กันรีเฟรช
  const data = new FormData(e.target)
  console.log("user =", data.get("user"))
})
```

---

## สร้าง `utilities.js` (อย่างน้อย 3 ฟังก์ชัน)

**เป้าหมาย:** รวมฟังก์ชันใช้บ่อยไว้ที่เดียว แล้วเรียกใช้จาก `app.js`

```js
// utilities.js
window.AppUtils = {
  toTitleCase(str) {
    return str.replace(/\w\S*/g, s => s[0].toUpperCase() + s.slice(1).toLowerCase())
  },
  clamp(num, min, max) {
    return Math.min(Math.max(num, min), max)
  },
  randomInt(min, max) {
    return Math.floor(Math.random() * (max - min + 1)) + min
  }
}
```

---

## ใช้งาน `utilities.js` ในหน้าเว็บ

```html
<script src="utilities.js" defer></script>
<script src="app.js" defer></script>
```

```js
// app.js
console.log(AppUtils.toTitleCase("hello world"))
console.log(AppUtils.clamp(120, 0, 100)) // 100
console.log(AppUtils.randomInt(1, 6))    // 1..6
```

---

## สาธิต Mini Task (5–10′)

* รับชื่อจาก `<input>` → แสดงผลด้วย `toTitleCase`
* ปุ่ม “สุ่มแต้ม 1–6” → เรียก `randomInt` แล้วแสดง `console.log` และบนหน้าเว็บ

---

## โปรเจกต์: เครื่องคิดเลข (เป้า)

* ปุ่มตัวเลข 0–9, จุดทศนิยม
* ปุ่ม `+ - × ÷ =` และ `C` (ล้าง)
* หน้าจอแสดงผล (display)
* รองรับ Bootstrap ที่เรียนมาแล้ว

---

## โครงสร้าง HTML (Calculator)

```html
<!-- index.html (ส่วนสำคัญ) -->
<link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/bootstrap@5/dist/css/bootstrap.min.css">
<div class="container py-4">
  <h1 class="h4 mb-3">Calculator</h1>
  <div class="card p-3" style="max-width: 360px">
    <input id="display" class="form-control mb-3 text-end" type="text" value="0" readonly>
    <div class="row g-2">
      <div class="col-3"><button class="btn btn-light w-100" data-num="7">7</button></div>
      <div class="col-3"><button class="btn btn-light w-100" data-num="8">8</button></div>
      <div class="col-3"><button class="btn btn-light w-100" data-num="9">9</button></div>
      <div class="col-3"><button class="btn btn-secondary w-100" data-op="divide">÷</button></div>

      <div class="col-3"><button class="btn btn-light w-100" data-num="4">4</button></div>
      <div class="col-3"><button class="btn btn-light w-100" data-num="5">5</button></div>
      <div class="col-3"><button class="btn btn-light w-100" data-num="6">6</button></div>
      <div class="col-3"><button class="btn btn-secondary w-100" data-op="multiply">×</button></div>

      <div class="col-3"><button class="btn btn-light w-100" data-num="1">1</button></div>
      <div class="col-3"><button class="btn btn-light w-100" data-num="2">2</button></div>
      <div class="col-3"><button class="btn btn-light w-100" data-num="3">3</button></div>
      <div class="col-3"><button class="btn btn-secondary w-100" data-op="minus">−</button></div>

      <div class="col-3"><button class="btn btn-light w-100" data-num="0">0</button></div>
      <div class="col-3"><button class="btn btn-light w-100" id="dot">.</button></div>
      <div class="col-3"><button class="btn btn-danger w-100" id="clear">C</button></div>
      <div class="col-3"><button class="btn btn-secondary w-100" data-op="plus">+</button></div>

      <div class="col-12"><button class="btn btn-primary w-100" id="equals">=</button></div>
    </div>
  </div>
</div>

<script src="utilities.js" defer></script>
<script src="app.js" defer></script>
```

---

## ตรรกะเครื่องคิดเลข (State Design)

* `current` (สตริงตัวเลขที่พิมพ์อยู่)
* `previous` (ค่าก่อนหน้า)
* `operator` (`plus/minus/multiply/divide` หรือ `null`)
* ฟังก์ชันช่วย:

  * `updateDisplay()`
  * `handleNumber(d)`
  * `handleOperator(op)`
  * `handleEquals()`
  * `handleClear()`

---

## โค้ด JS (Calculator – ส่วนที่ 1)

```js
// app.js
const display = document.getElementById("display")
let current = "0"
let previous = null
let operator = null

function updateDisplay() {
  display.value = current
}
function handleNumber(d) {
  if (current === "0") current = d
  else current += d
  updateDisplay()
}
document.querySelectorAll("[data-num]").forEach(btn => {
  btn.addEventListener("click", () => handleNumber(btn.dataset.num))
})
document.getElementById("dot").addEventListener("click", () => {
  if (!current.includes(".")) current += "."
  updateDisplay()
})
```

---

## โค้ด JS (Calculator – ส่วนที่ 2)

```js
function handleOperator(op) {
  if (previous === null) {
    previous = parseFloat(current)
  } else if (operator) {
    // คำนวณค้าง
    previous = compute(previous, parseFloat(current), operator)
  }
  operator = op
  current = "0"
  updateDisplay()
}

function compute(a, b, op) {
  switch (op) {
    case "plus": return a + b
    case "minus": return a - b
    case "multiply": return a * b
    case "divide": return b === 0 ? NaN : a / b
    default: return b
  }
}
document.querySelectorAll("[data-op]").forEach(btn => {
  btn.addEventListener("click", () => handleOperator(btn.dataset.op))
})
```

---

## โค้ด JS (Calculator – ส่วนที่ 3)

```js
function handleEquals() {
  if (operator === null || previous === null) return
  const result = compute(previous, parseFloat(current), operator)
  current = String(result)
  previous = null
  operator = null
  updateDisplay()
}
function handleClear() {
  current = "0"
  previous = null
  operator = null
  updateDisplay()
}

document.getElementById("equals").addEventListener("click", handleEquals)
document.getElementById("clear").addEventListener("click", handleClear)

// เริ่มต้น
updateDisplay()
```

---

## เพิ่มคุณภาพ UX (ทางเลือก)

* กดแป้นพิมพ์: map `keydown` → เลข/./+-*/Enter/Escape
* แสดงเลขแบบอ่านง่าย: ใช้ `Intl.NumberFormat()` เมื่ออัปเดตจอ
* ป้องกัน overflow: `current.length` สูงสุด / ใช้ `clamp` จาก `utilities.js`

---

## แบบฝึกหัดระหว่างเรียน

1. เขียน `max3(a,b,c)` คืนค่ามากสุด
2. เขียน `isEven(n)` คืนค่า boolean
3. เพิ่มปุ่ม `%` ให้เครื่องคิดเลข (เปอร์เซ็นต์ของตัวปัจจุบัน)
4. ใช้ `console.table()` แสดงรายการตัวเลขที่กดล่าสุด 5 รายการ

---

## Debugging Tips

* ใช้ `console.log()` ชี้จุดผิด
* อ่าน error message ให้ครบ
* แยกโค้ดเป็นฟังก์ชันเล็ก ๆ ทดสอบทีละส่วน
* ใช้ Breakpoints ใน DevTools (Tab **Sources**)

---

## Recap

* ตัวแปร/ชนิดข้อมูล/Operator/Control Flow
* ฟังก์ชัน & Scope
* Console และการดีบัก
* DOM & Events
* `utilities.js` ที่เรียกใช้ซ้ำ
* สร้างเครื่องคิดเลขพร้อม Bootstrap

---

## งานที่ควรมีในโฟลเดอร์ส่ง

* `index.html` (Calculator UI + ลิงก์ JS)
* `utilities.js` (≥ 3 ฟังก์ชัน)
* `app.js` (ตรรกะเครื่องคิดเลข + event)
* (ถ้ามี) `styles.css`

---

## การบ้าน/ต่อยอด

* รองรับคีย์บอร์ดเต็มรูปแบบ
* ปุ่ม `±` (สลับเครื่องหมาย)
* ประวัติการคำนวณ (History)
* หน่วยความจำ `MC/MR/M+/M-`
* เขียน **Unit Tests เบื้องต้น** ให้ `compute()` (ถ้าเรียนต่อ JS Testing)

---

## Q&A

พร้อมลองเขียน เพิ่มปุ่มใหม่ หรือแก้บั๊กเล็ก ๆ ไปด้วยกัน! ✨

```

อยากให้ปรับธีม สี โทน หรือเพิ่มโลโก้สถาบันในสไลด์บอกได้เลย เดี๋ยวผมจัดให้ 🙂
```
