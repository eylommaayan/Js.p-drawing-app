<img width="843" height="796" alt="image" src="https://github.com/user-attachments/assets/4c5d7d89-2277-4005-8443-eb3265800bfa" />

<img width="881" height="853" alt="image" src="https://github.com/user-attachments/assets/8ca95490-2858-4b30-9119-c87979d3872d" />

<img width="856" height="834" alt="image" src="https://github.com/user-attachments/assets/c54b9a93-8623-4b05-a906-eb7e6e9e04e2" />


 



להלן README מוכן לפרויקט “ציור ב-Canvas” — מסביר בקצרה מה לומדים ב-JS ובקוד שהבאת:

---

# Canvas Drawing App — מדריך ו-README

## מה הפרויקט עושה?

אפליקציית ציור פשוטה בתוך `<canvas>` בדפדפן: בוחרים צבע, מגדילים/מקטינים את מברשת, מציירים עיגולים וקווים רציפים עם העכבר, ומנקים את הלוח בלחיצה.

## מה לומדים כאן ב-JavaScript?

* **שליפת אלמנטים מה-DOM** עם `getElementById`.
* **האזנה לאירועים**: `mousedown`, `mouseup`, `mousemove`, ו-`click` על כפתורים.
* **ניהול מצב (state) בסיסי**: משתנים גלובליים כמו `isPressed`, `size`, `color`, `x`, `y`.
* **עבודה עם Canvas 2D API**:
  `getContext('2d')`, ציור **עיגולים** (`beginPath`, `arc`, `fill`) ו**קווים** (`moveTo`, `lineTo`, `stroke`).
* **סגנון מברשת**: שימוש ב-`fillStyle`, `strokeStyle`, ו-`lineWidth`.
* **אינטרפולציה בין נקודות**: קישור בין הנקודה הקודמת לנוכחית כדי לקבל קו חלק תוך כדי גרירה.
* **בקרת קלט**: שינוי צבע דרך `<input type="color">` ועדכון גודל מברשת עם כפתורים.
* **ניקוי הקנבס**: `clearRect`.

## איך זה עובד (בגדול)?

1. כשנלחץ עכבר על הקנבס (`mousedown`) שומרים את המיקום ההתחלתי ומפעילים מצב ציור (`isPressed = true`).
2. בעת גרירת עכבר (`mousemove`) ובתנאי שמצויר, מציירים:

   * **עיגול** במיקום הנוכחי — יוצר “מברשת” עגולה.
   * **קו** בין המיקום הקודם לנוכחי — יוצר רצף חלק.
     לבסוף מעדכנים את `x,y` לנקודה הנוכחית.
3. בשחרור העכבר (`mouseup`) מכבים מצב ציור ומאפסים את הקואורדינטות.
4. כפתורי `+`/`-` משנים את `size` בתחום 5–50 ומעדכנים תצוגה.
5. בורר הצבע מעדכן את `color`.
6. כפתור Clear מנקה את כל הציור עם `clearRect`.

## קטעי קוד חשובים

### אתחול הקנבס והקשר גרפי

```js
const canvas = document.getElementById('canvas');
const ctx = canvas.getContext('2d');
```

### ציור “מברשת” (עיגול)

```js
function drawCircle(x, y) {
  ctx.beginPath();
  ctx.arc(x, y, size, 0, Math.PI * 2);
  ctx.fillStyle = color;
  ctx.fill();
}
```

### קו מחבר בין נקודות

```js
function drawLine(x1, y1, x2, y2) {
  ctx.beginPath();
  ctx.moveTo(x1, y1);
  ctx.lineTo(x2, y2);
  ctx.strokeStyle = color;
  ctx.lineWidth = size * 2;
  ctx.stroke();
}
```

### אירועי עכבר (ליבה של הלוגיקה)

```js
let isPressed = false, x, y;

canvas.addEventListener('mousedown', (e) => {
  isPressed = true;
  x = e.offsetX; y = e.offsetY;
});

document.addEventListener('mouseup', () => {
  isPressed = false;
  x = undefined; y = undefined;
});

canvas.addEventListener('mousemove', (e) => {
  if (!isPressed) return;
  const x2 = e.offsetX, y2 = e.offsetY;
  drawCircle(x2, y2);
  drawLine(x, y, x2, y2);
  x = x2; y = y2;
});
```

## מבנה HTML מינימלי

כדי שה-JS יעבוד, ודאו שיש אלמנטים עם אותם מזהים (IDs):

```html
<div class="toolbox">
  <button id="decrease">-</button>
  <span id="size">10</span>
  <button id="increase">+</button>
  <input type="color" id="color" />
  <button id="clear">X</button>
</div>

<canvas id="canvas" width="800" height="600"></canvas>
```

> **טיפ:** ודאו שה-CSS של `.toolbox` והמסגרת של ה-`canvas` מוטמעים (כמו בקוד שסיפקת) כדי לקבל פריסה נוחה.

## איך מפעילים?

1. שימו את קבצי ה-HTML, CSS ו-JS באותה תיקייה.
2. פתחו את קובץ ה-`index.html` בדפדפן מודרני (Chrome/Edge/Firefox).
3. ציירו עם העכבר! החליפו צבע/גודל בעזרת הכלים.
.

## דגשים ופתרון תקלות

* אם אין ציור: ודאו שה-`canvas` אכן קיבל **width/height** באטריביוטים (לא רק ב-CSS).
* אם הקו “מרובע”: נסו להוסיף:

  ```js
  ctx.lineCap = 'round';
  ctx.lineJoin = 'round';
  ```
* אם הצבע לא מתעדכן: בדקו שה-`input[type="color"]` קיים וש-`colorEl.addEventListener('change', ...)` רץ אחרי טעינת ה-DOM.

## רישוי

פרויקט לימודי — אפשר להשתמש, להעתיק, ולהרחיב חופשי (הוסיפו רישוי לבחירתכם).



