# ชุดใบงาน 8 สัปดาห์: Mini CRUD + Google Sheets API (ปี 1 เทอม 1)

> เป้าหมาย: ผู้เรียนสร้างเว็บหน้าเดียว (Single Page แบบง่าย) ที่ **แสดงรายการ (GET)** และ **เพิ่มข้อมูล (POST)** ผ่าน Web API ที่เชื่อม Google Sheets ซึ่งเตรียมให้ พร้อม UI responsive ด้วย Bootstrap และสถานะใช้งานครบ (loading/empty/error)

> โครงสร้างโปรเจ็กต์พื้นฐาน ใช้ชุดไฟล์: `index.html`, `styles.css`, `app.js`

> สัญลักษณ์ในใบงาน:
> **[Starter]** โค้ดตั้งต้นสำหรับเริ่มทำในสัปดาห์นั้น
> **[Tasks]** ภารกิจที่ต้องทำ
> **[API]** ตัวอย่างคำขอ/คำตอบ API (ใช้โครงสร้างนี้กับ API จริงที่จัดให้)
> **[Checklist]** เช็กลิสต์ส่งงาน
> **[Bonus]** งานเสริม (ถ้ามีเวลา)

---

## สัปดาห์ที่ 1 — HTML + Bootstrap Layout & Forms

**วัตถุประสงค์**

* สร้างหน้าเว็บโครงสร้าง semantic + Bootstrap Grid/Utilities
* วางฟอร์มกรอกข้อมูล (ยังไม่เชื่อม API) และตารางแสดงรายการ (ยังใส่ข้อมูลจำลอง)

**[Starter]** `index.html`

```html
<!doctype html>
<html lang="th">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <title>Mini CRUD Starter</title>
  <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/css/bootstrap.min.css" rel="stylesheet">
  <link rel="stylesheet" href="styles.css" />
</head>
<body>
  <nav class="navbar navbar-expand-lg navbar-dark bg-dark">
    <div class="container"><a class="navbar-brand" href="#">Mini CRUD</a></div>
  </nav>

  <main class="container py-4">
    <div class="row g-4">
      <section class="col-12 col-lg-4">
        <h2 class="h5 mb-3">เพิ่มรายการ</h2>
        <form id="create-form" class="card card-body shadow-sm">
          <div class="mb-3">
            <label class="form-label">ชื่อรายการ</label>
            <input name="title" class="form-control" placeholder="เช่น หนังสือเล่มใหม่" required />
          </div>
          <div class="mb-3">
            <label class="form-label">รายละเอียด</label>
            <textarea name="note" class="form-control" rows="3" placeholder="บันทึกเพิ่มเติม"></textarea>
          </div>
          <button class="btn btn-primary" type="submit">บันทึก (ยังไม่เชื่อม API)</button>
        </form>
      </section>

      <section class="col-12 col-lg-8">
        <div class="d-flex align-items-center gap-2 mb-2">
          <h2 class="h5 m-0">รายการข้อมูล</h2>
          <span class="badge bg-secondary">สัปดาห์ 1: ข้อมูลจำลอง</span>
        </div>
        <div class="table-responsive shadow-sm">
          <table class="table table-striped align-middle" id="data-table">
            <thead class="table-light">
              <tr>
                <th>#</th><th>ชื่อรายการ</th><th>รายละเอียด</th>
              </tr>
            </thead>
            <tbody>
              <tr><td>1</td><td>ตัวอย่าง A</td><td>ข้อมูลจำลอง</td></tr>
              <tr><td>2</td><td>ตัวอย่าง B</td><td>ข้อมูลจำลอง</td></tr>
            </tbody>
          </table>
        </div>
      </section>
    </div>
  </main>

  <script src="app.js"></script>
</body>
</html>
```

**[Starter]** `styles.css`

```css
body { background: #f8f9fa; }
```

**[Starter]** `app.js`

```js
// สัปดาห์ 1: ยังไม่เขียน JS จริงจัง แค่กัน reload ฟอร์ม
document.getElementById('create-form').addEventListener('submit', (e) => {
  e.preventDefault();
  alert('สัปดาห์ 1: ยังไม่เชื่อม API / ยังไม่เพิ่มลงตาราง');
});
```

**[Tasks]**

1. จัด layout ให้สวยและอ่านง่ายในมือถือ/เดสก์ท็อป (ลองย่อ-ขยายหน้าจอ)
2. เพิ่มช่องฟอร์มอื่น ๆ ที่จำเป็นกับโดเมนที่เลือก (เช่น ราคา/หมวดหมู่)
3. สร้างตารางหัวข้อครบ และข้อมูลจำลอง 3–5 แถว

**[Checklist]**

* [ ] โครงสร้าง semantic + Bootstrap Grid/Utilities ใช้งานได้
* [ ] ฟอร์มและตารางอ่านง่ายในจอเล็ก
* [ ] ไม่มี error ในคอนโซล

**[Bonus]**

* ใช้ `placeholder`, `required` กับ input อย่างเหมาะสม

---

## สัปดาห์ที่ 2 — JavaScript เบื้องต้น, DOM & Events

**วัตถุประสงค์**

* เข้าใจตัวแปร/Array/Object/ฟังก์ชัน
* อ่านค่าฟอร์มและ **เพิ่มรายการลงตารางจาก Array ในหน่วยความจำ (ยังไม่ใช้ API)**

**[Starter]** `app.js`

```js
const state = {
  items: [
    { id: 1, title: 'ตัวอย่าง A', note: 'ข้อมูลจำลอง' },
    { id: 2, title: 'ตัวอย่าง B', note: 'ข้อมูลจำลอง' }
  ],
  nextId: 3
};

function renderTable(items) {
  const tbody = document.querySelector('#data-table tbody');
  tbody.innerHTML = items.map((it, idx) => `
    <tr>
      <td>${idx + 1}</td>
      <td>${it.title}</td>
      <td>${it.note ?? ''}</td>
    </tr>
  `).join('');
}

renderTable(state.items);

document.getElementById('create-form').addEventListener('submit', (e) => {
  e.preventDefault();
  const fd = new FormData(e.currentTarget);
  const item = { id: state.nextId++, title: fd.get('title'), note: fd.get('note') };
  state.items.push(item);
  renderTable(state.items);
  e.currentTarget.reset();
});
```

**[Tasks]**

1. เติมการตรวจสอบอินพุตง่าย ๆ (ห้ามชื่อว่าง)
2. เคลียร์ฟอร์มและโฟกัสกลับช่องแรกหลังเพิ่ม

**[Checklist]**

* [ ] เพิ่มรายการแล้วแสดงบนตารางทันที
* [ ] ไม่มีการรีโหลดหน้า

**[Bonus]**

* แสดงข้อความเตือนเล็ก ๆ ใต้ช่องถ้าไม่ผ่านเงื่อนไข

---

## สัปดาห์ที่ 3 — ค้นหา/กรอง + สถานะว่าง (Empty)

**วัตถุประสงค์**

* เพิ่มช่องค้นหาและกรองรายการจาก `state.items`
* แสดงสถานะ **Empty** เมื่อไม่มีรายการ

**[Starter]** เพิ่มช่องค้นหาใน `index.html`

```html
<div class="input-group mb-3">
  <span class="input-group-text">ค้นหา</span>
  <input id="search" class="form-control" placeholder="พิมพ์คำค้น..." />
</div>
```

**[Starter]** ปรับ `app.js`

```js
const searchEl = document.getElementById('search');

function applyFilter() {
  const q = (searchEl.value || '').toLowerCase();
  const filtered = state.items.filter(it =>
    it.title.toLowerCase().includes(q) || (it.note||'').toLowerCase().includes(q)
  );
  renderTable(filtered);
  document.querySelector('#data-table tbody').closest('.table-responsive').dataset.count = filtered.length;
  showEmptyIfNeeded(filtered.length === 0);
}

function showEmptyIfNeeded(isEmpty) {
  const tbody = document.querySelector('#data-table tbody');
  if (isEmpty) {
    tbody.innerHTML = `<tr><td colspan="3" class="text-center text-muted">ไม่มีข้อมูล</td></tr>`;
  }
}

renderTable(state.items);
applyFilter();
searchEl.addEventListener('input', applyFilter);
```

**[Tasks]**

1. ทำให้ค้นหาได้ทั้งชื่อและรายละเอียด (ตามตัวอย่าง)
2. ถ้าไม่มีรายการ ให้แสดงแถว "ไม่มีข้อมูล"

**[Checklist]**

* [ ] ค้นหาแล้วตารางอัปเดตทันที
* [ ] แสดงสถานะว่างได้ถูกต้อง

**[Bonus]**

* ไฮไลต์ข้อความที่ตรงกับคำค้น

---

## สัปดาห์ที่ 4 — เชื่อมต่อ API (GET) + แสดงสถานะ Loading/Error

**วัตถุประสงค์**

* ใช้ `fetch` เรียก **GET** จาก Web API (Google Sheets)
* จัดการสถานะ **loading**/**error** และแทนที่ข้อมูลจำลองด้วยข้อมูลจริง

**[Starter]** เพิ่มพื้นที่แถบสถานะใน `index.html`

```html
<div id="status" class="alert d-none" role="alert"></div>
```

**[Starter]** `app.js` เชื่อม API (แทนค่า URL จริงที่ได้รับ)

```js
const API_BASE = '<<PUT_YOUR_API_BASE_URL>>'; // เช่น https://script.google.com/macros/s/XXXX/exec

async function loadData() {
  setStatus('กำลังโหลดข้อมูล...', 'info');
  try {
    const res = await fetch(`${API_BASE}/items`, { method: 'GET' });
    if (!res.ok) throw new Error('โหลดข้อมูลล้มเหลว');
    const data = await res.json();
    state.items = Array.isArray(data.items) ? data.items : [];
    renderTable(state.items);
    setStatus('', '');
    applyFilter();
  } catch (err) {
    setStatus('เกิดข้อผิดพลาดในการโหลดข้อมูล', 'danger');
    console.error(err);
  }
}

function setStatus(msg, type) {
  const el = document.getElementById('status');
  if (!msg) { el.className = 'alert d-none'; el.textContent = ''; return; }
  el.className = `alert alert-${type}`;
  el.textContent = msg;
}

loadData();
```

**[API]** ตัวอย่างโครงสร้างคำตอบ (GET `/items`)

```json
{
  "items": [
    { "id": 101, "title": "หนังสือ", "note": "โน้ตสั้นๆ" },
    { "id": 102, "title": "ปากกา", "note": "สีน้ำเงิน" }
  ]
}
```

**[Tasks]**

1. แทน URL จริงของ API ที่ได้รับ
2. แสดงสถานะกำลังโหลดและซ่อนเมื่อเสร็จ
3. แสดง error เมื่อการเชื่อมต่อล้มเหลว

**[Checklist]**

* [ ] ดึงข้อมูลจาก API จริงได้
* [ ] มีสถานะ loading และ error ที่เข้าใจง่าย

**[Bonus]**

* แสดงจำนวนรายการทั้งหมดที่โหลดได้

---

## สัปดาห์ที่ 5 — ส่งข้อมูลขึ้น API (POST) + รีโหลดตาราง

**วัตถุประสงค์**

* อ่านค่าฟอร์ม ส่ง **POST** ไปที่ API
* หลังบันทึกสำเร็จ เรียก `loadData()` เพื่อรีเฟรชตาราง

**[Starter]** ปรับ `app.js`

```js
document.getElementById('create-form').addEventListener('submit', async (e) => {
  e.preventDefault();
  const fd = new FormData(e.currentTarget);
  const payload = {
    title: (fd.get('title')||'').trim(),
    note: (fd.get('note')||'').trim()
  };
  if (!payload.title) { alert('กรุณากรอกชื่อรายการ'); return; }
  setStatus('กำลังบันทึก...', 'info');
  try {
    const res = await fetch(`${API_BASE}/items`, {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify(payload)
    });
    if (!res.ok) throw new Error('บันทึกไม่สำเร็จ');
    const result = await res.json();
    // คาดหวัง { success: true, id: 123 }
    e.currentTarget.reset();
    setStatus('บันทึกสำเร็จ', 'success');
    await loadData();
  } catch (err) {
    console.error(err);
    setStatus('เกิดข้อผิดพลาดขณะบันทึก', 'danger');
  }
});
```

**[API]** ตัวอย่างคำขอ/คำตอบ (POST `/items`)

```http
POST /items
Content-Type: application/json

{ "title": "ดินสอ", "note": "HB" }
```

ตอบกลับ

```json
{ "success": true, "id": 201 }
```

**[Tasks]**

1. ตรวจสอบค่าว่างก่อนส่ง
2. แสดงข้อความสำเร็จ/ผิดพลาด
3. โหลดข้อมูลใหม่หลังเพิ่มเสร็จ

**[Checklist]**

* [ ] เพิ่มรายการใหม่ผ่าน API ได้จริง
* [ ] ตารางอัปเดตหลังเพิ่ม

**[Bonus]**

* ปิดปุ่ม Submit ระหว่างกำลังส่ง เพื่อป้องกันการกดซ้ำ

---

## สัปดาห์ที่ 6 — ปรับ UX: Debounced Search + สถานะ Empty แยกชัดเจน

**วัตถุประสงค์**

* ทำช่องค้นหาพร้อม debounce (หน่วงก่อนค้นหา)
* แยกส่วนแสดง **empty state** ชัดเจน

**[Starter]** ปรับ `app.js` (เสริม debounce)

```js
function debounce(fn, delay = 300){
  let t; return (...args) => { clearTimeout(t); t = setTimeout(() => fn(...args), delay); };
}
const onSearchChanged = debounce(applyFilter, 250);
searchEl.removeEventListener('input', applyFilter);
searchEl.addEventListener('input', onSearchChanged);
```

**[Starter]** สร้างพื้นที่ empty แยกใน `index.html`

```html
<div id="empty" class="text-center text-muted py-4 d-none">ไม่มีข้อมูลที่จะแสดง</div>
```

**[Starter]** ปรับ `showEmptyIfNeeded`

```js
function showEmptyIfNeeded(isEmpty){
  const empty = document.getElementById('empty');
  const table = document.getElementById('data-table');
  empty.classList.toggle('d-none', !isEmpty);
  table.classList.toggle('d-none', isEmpty);
}
```

**[Tasks]**

1. ค้นหาแบบหน่วง 250ms
2. ซ่อน/แสดงตารางกับ empty อย่างเหมาะสม

**[Checklist]**

* [ ] ประสบการณ์ค้นหา ลื่นกว่าเดิม
* [ ] สถานะว่างอ่านง่าย

**[Bonus]**

* แสดงจำนวนผลลัพธ์ค้นหาแบบเรียลไทม์

---

## สัปดาห์ที่ 7 — (เสริม) Update/Delete ผ่าน API + ยืนยันการลบ

**วัตถุประสงค์**

* เพิ่มปุ่ม **แก้ไข/ลบ** ต่อแถว (ถ้าทำทัน)
* ใช้ **PUT/DELETE** กับ API

**[Starter]** ปรับหัวตารางและแถวใน `renderTable`

```js
function renderTable(items){
  const tbody = document.querySelector('#data-table tbody');
  tbody.innerHTML = items.map((it, idx) => `
    <tr>
      <td>${idx + 1}</td>
      <td>${it.title}</td>
      <td>${it.note ?? ''}</td>
      <td class="text-end">
        <button class="btn btn-sm btn-outline-secondary" data-action="edit" data-id="${it.id}">แก้ไข</button>
        <button class="btn btn-sm btn-outline-danger" data-action="del" data-id="${it.id}">ลบ</button>
      </td>
    </tr>`).join('');
}

document.querySelector('#data-table').addEventListener('click', async (e) => {
  const btn = e.target.closest('button[data-action]');
  if(!btn) return;
  const id = btn.dataset.id;
  const action = btn.dataset.action;
  if(action === 'del'){
    if(!confirm('ยืนยันการลบ?')) return;
    try{
      const res = await fetch(`${API_BASE}/items/${id}`, { method: 'DELETE' });
      if(!res.ok) throw new Error('ลบไม่สำเร็จ');
      await loadData();
    }catch(err){ setStatus('ลบไม่สำเร็จ', 'danger'); }
  }
  // หมายเหตุ: ส่วนแก้ไข (edit) สามารถทำเป็น modal + PUT เช่นเดียวกัน
});
```

**[API]** ตัวอย่าง

```http
DELETE /items/201  ->  { "success": true }
PUT /items/201     ->  { "success": true, "item": { "id":201, "title":"ใหม่", "note":"แก้ไขแล้ว" } }
```

**[Tasks]**

1. เพิ่มปุ่มลบและยืนยันก่อนลบ
2. (เสริม) ทำฟอร์มแก้ไขใน modal และส่ง PUT

**[Checklist]**

* [ ] ลบข้อมูลได้จริงและรีโหลดตาราง
* [ ] (เสริม) แก้ไขได้จริง

**[Bonus]**

* Optimistic UI (ลบ/แก้ไขบนจอทันที แล้วค่อยยืนยันผลจาก API)

---

## สัปดาห์ที่ 8 — ดีพลอย GitHub Pages + README + ตรวจความพร้อม

**วัตถุประสงค์**

* เผยแพร่หน้าเว็บด้วย GitHub Pages
* เขียน README อธิบายการใช้งาน และแนบ URL API ที่ต้องตั้งค่า

**[Starter]** โครง `README.md`

````md
# Mini CRUD (Student Name)

## วิธีใช้งาน
1. เปิดหน้าเว็บจากลิงก์: <URL GitHub Pages>
2. ใส่ข้อมูลในฟอร์มแล้วกดบันทึก
3. รายการจะปรากฏในตารางทันที

## การตั้งค่า API
- ตั้งค่าตัวแปร `API_BASE` ใน `app.js` ให้ชี้ไปยัง Web API ที่ได้รับ
- รูปแบบข้อมูล (GET /items)
```json
{ "items": [ { "id": 1, "title": "ตัวอย่าง", "note": "..." } ] }
````

## เทคโนโลยีที่ใช้

* HTML, CSS, Bootstrap 5, JavaScript (fetch)

## ผู้จัดทำ

* ชื่อ-นามสกุล, รหัสนักศึกษา

````

**[Tasks]**  
1) สร้าง repo และเปิด GitHub Pages (Branch `main` → `/root`)  
2) ตรวจการทำงานบนมือถือจริง / DevTools mobile view  
3) เติม README ให้ชัดเจน  

**[Checklist]**  
- [ ] ลิงก์ GitHub Pages ใช้งานได้  
- [ ] แสดง/เพิ่มข้อมูลผ่าน API ได้จากลิงก์สาธารณะ  
- [ ] README ครบถ้วน  

**[Bonus]**  
- เพิ่มรูป screenshot หน้าจอการทำงานใน README

---

# ภาคผนวก A — รูปแบบ API โดยสรุป (ใช้เป็นแนวทาง)
> ให้ใช้ URL จริงแทนที่ `<<PUT_YOUR_API_BASE_URL>>` ตามที่ผู้สอนจัดเตรียม

- **GET** `/items` → ส่งคืน
```json
{ "items": [ { "id": 1, "title": "ตัวอย่าง", "note": "ข้อความ" } ] }
````

* **POST** `/items`  (ส่ง `application/json`)

```json
{ "title": "ข้อความ", "note": "เพิ่มเติม" }
```

ตอบกลับ

```json
{ "success": true, "id": 999 }
```

* **DELETE** `/items/{id}` → ส่งคืน

```json
{ "success": true }
```

* **PUT** `/items/{id}` (เสริม)

```json
{ "title": "แก้ไขแล้ว", "note": "ปรับปรุง" }
```

ตอบกลับ

```json
{ "success": true, "item": { "id": 1, "title": "แก้ไขแล้ว", "note": "ปรับปรุง" } }
```

---

# ภาคผนวก B — เช็กลิสต์รวมก่อนส่งโปรเจ็กต์สุดท้าย

* [ ] หน้าเดียว: ตาราง + ฟอร์ม + ค้นหา
* [ ] GET/POST ทำงานกับ API จริง
* [ ] สถานะ UI: loading/empty/error
* [ ] รองรับมือถือ (Bootstrap)
* [ ] โครงสร้างไฟล์สะอาด, ชื่อสื่อความ
* [ ] README ครบ + ลิงก์ดีพลอยใช้งานได้
* [ ] (ถ้ามี) PUT/DELETE ผ่านได้
