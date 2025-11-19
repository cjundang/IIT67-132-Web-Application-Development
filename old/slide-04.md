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

# JavaScript เบื้องต้น  
## (สำหรับนักศึกษาปี 1 – ผ่าน HTML/CSS/Bootstrap มาแล้ว)

**วัตถุประสงค์**
- เข้าใจตัวแปร ชนิดข้อมูล ตัวดำเนินการ และโครงสร้างควบคุม  
- เขียนฟังก์ชันและเข้าใจ scope เบื้องต้น  
- ใช้ DevTools Console ทดสอบโค้ด  
- จัดการ event/action บนหน้าเว็บ  
- สร้างไฟล์ `utilities.js` (≥ 3 ฟังก์ชัน)  
---

## ก่อนเริ่ม: เตรียมเครื่องมือ

- Browser (Chrome/Edge/Firefox) + **DevTools (Console)**
- VS Code หรือ Editor ที่ถนัด  
- โครงไฟล์งานวันนี้  
```

/--js-basics
 |-index.html
 |-app.js
 |-utilities.js
 |-styles.css (ถ้ามี)

````

---

## JavaScript คืออะไร?

- ภาษาสคริปต์สำหรับเว็บ: ทำให้หน้า **โต้ตอบได้**  
- รันได้ใน Browser และนอก Browser (Node.js)  
- วันนี้โฟกัส **ฝั่ง Browser** + DevTools Console

---

## เพิ่ม JS ลงหน้าเว็บ (ทบทวน)

**วิธีแนะนำ (External):**
```html
<script src="app.js" defer></script>
````

* ใช้ `defer` เพื่อให้ DOM โหลดเสร็จก่อนรันสคริปต์
* หลีกเลี่ยง inline script เพื่อการแยก concerns

---

## DevTools Console (สาธิต)

* เปิด DevTools → Tab **Console**
* ลองพิมพ์:

```js
1 + 2
console.log("Hello JS")
```

* ฟังก์ชันช่วยดีบัก: `console.log()`, `console.error()`, `console.warn()`, `console.table()`, `console.dir()`

---

## ตัวแปร (Variables)

* `let` และ `const` (แนะนำ) / `var` (เลี่ยง)

```js
let count = 0
const PI = 3.14159
// PI = 4  // ❌ error
```

**หลักการ:** ใช้ `const` เป็นค่าเริ่มต้น ถ้าต้องเปลี่ยนค่า ให้ใช้ `let`

---

## ชนิดข้อมูลพื้นฐาน (Data Types)

* number, string, boolean, null, undefined, object, array

```js
typeof 42           // "number"
typeof "hello"      // "string"
typeof true         // "boolean"
typeof null         // "object" (legacy quirk)
typeof undefined    // "undefined"
typeof {}           // "object"
Array.isArray([])   // true
```

* Template literals: `` `Hi, ${name}!` ``

---

## Operator (ตัวดำเนินการ)

* เลขคณิต: `+ - * / % **`
* เปรียบเทียบ: `> < >= <= == === != !==`
* ตรรกะ: `&& || !`
* กำหนดค่า: `= += -= *= /=`
  **จำไว้:** ใช้ `===` / `!==` แทน `==` / `!=`

```js
"2" == 2   // true  (แปลงชนิดอัตโนมัติ)
"2" === 2  // false (ตรวจทั้งค่าและชนิด)
```

---

## แบบฝึกหัดสั้น (Console)

1. ประกาศ `let a = 10`, `let b = "5"` แล้วลอง `a + b`, `a + Number(b)`
2. สร้างตัวแปร `temperature` แล้วพิมพ์ข้อความ `Today: X °C` ด้วย template literal

---

## Control Flow: if / else / switch

```js
const score = 78
if (score >= 80) {
  console.log("A")
} else if (score >= 70) {
  console.log("B")
} else {
  console.log("C or lower")
}
```

```js
const role = "admin"
switch (role) {
  case "admin": console.log("Full access"); break
  case "user":  console.log("Limited access"); break
  default:      console.log("Guest")
}
```

---

## Loop เบื้องต้น

```js
for (let i = 0; i < 3; i++) console.log(i)

const arr = [10, 20, 30]
for (const n of arr) console.log(n)   // ค่าจาก array

const obj = { name: "A", age: 18 }
for (const k in obj) console.log(k, obj[k]) // key จาก object
```

---

## Functions (ฟังก์ชัน)

```js
// ประกาศแบบปกติ
function add(a, b) {
  return a + b
}

// ฟังก์ชันนิพจน์
const mul = function (a, b) {
  return a * b
}

// Arrow function
const sub = (a, b) => a - b
```

* Default params: `function greet(name = "Guest") { ... }`

---

## Scope & Hoisting (เบื้องต้น)

* **Block scope**: `let`, `const` มีขอบเขตใน `{ }`
* **Function scope**: `var`
* Hoisting: การยกประกาศตัวแปร/ฟังก์ชันไปบนสุด (แต่ค่าจะยังไม่ถูกกำหนด)

```js
console.log(x) // undefined (var hoist)
var x = 10

// function declaration hoist ได้ทั้งบล็อก
foo() // OK
function foo() { console.log("hi") }
```

