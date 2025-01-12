#### 1. `/<a\s+[^>]*>/` (crawler/crawler.py บรรทัดที่ 47)

แกะ regex:

ใช้สำหรับดึงข้อมูลจากแท็ก `<a>` ที่อยู่ใน HTML response ออกมาหลาย ๆ แท็ก โดยที่แท็ก `<a>` เป็นแท็กที่ใช้เชื่อมโยงไปยังหน้าเว็บไซต์อื่น ๆ หรือไฟล์อื่น ๆ ที่เกี่ยวข้องกับเนื้อหาในหน้าเว็บไซต์ปัจจุบัน

- `\s` คือ Matches any space, tab, or newline character

- `+` คือ รับทุกตัวก่อนหน้าที่ต่อจากนี้ 1 ตัว จนถึง infinity ตัว (คือรับ space ไม่จำกัด)

- `[^>]` คือ รับทุกตัวยกเว้น `>`

- `*` คือ รับทุกตัวต่อท้ายตั้งแต่จำนวน 0 ตัว ถึง infinity ตัว

- `[^>]*` แปลได้ว่า รับทุกตัวที่ไม่ใช่ `>` จำนวนตัวอักษรตั้งแต่ 0 และจะหยุดที่ `>`

สรุปโดยสั้น ๆ `/<a\s+[^>]*>/` ใช้สำหรับดึงข้อมูลจากแท็ก `<a>` ทั้งกรอบ `<>`

#### 2. `/(?<=href=").*?(?=")/` (crawler/crawler.py บรรทัดที่ 63)

แกะ regex:

ใช้เมื่อเราต้องการดึงข้อมูล link ของแต่ละจังหวัดจาก HTML response ที่ได้รับมา เพื่อนำไปใช้งานต่อไป

- `(?<=...)` Positive lookbehind คือ รับทุก quantifiers แต่ไม่นับ ตัวหลังเครื่องหมาย `?=`

- `(?<=href=")` คือ รับตัวหลัง `href="`

- `.` คือ รับตัวใดตัวนึง ในที่นี้ใช้ `.*` คือการรับตัวใดตัวนึงตั้งแต่ 0 ถึง n(ตามนิยามของ \*)

- `(?=")` คือ Positive lookahead รับตัวก่อน หลังเครื่องหมาย `?=` ซึ่งในที่นี้คือ `"`

สรุป จาก tag link จะคัดเลือกข้อมูลให้เหลือเพียง ลิ้งข้างใน href=”\_\_\_\_”

#### 3. `/(?<=title=").*?(?=")/` (crawler/crawler.py บรรทัดที่ 79)

ใช้สำหรับดึงเอาข้อมูลในสตริงที่อยู่ระหว่าง `title="` และ `"` ออกมา ซึ่งเป็นชื่อจังหวัดที่อยู่ในแท็ก `<a>` ใน HTML response ที่ได้รับมา

แกะ regex:

- `(?<=...)` Positive Lookbehind เลือกข้อมูลข้างหลัง… ในโค้ดคือ `(?<=title=")` เลือกข้อมูลหลังคำว่า `title=”`

- `.` Meta Escape คือ รับทุกตัวยกเว้น newline

- `*` Repetition Operator ในโค้ดคือ .\* เพื่อรับอักขระตัวใดก็ได้ ยกเว้น newline(“\n”) เป็นจำนวน 0 ถึง n ตัว

- `*?` Non-greedy Quantifier ในโค้ดคือ .\*? เพื่อรับสตริงที่ตรงเงื่อนไข ที่สั้นที่สุด

- `(?=...)` Positive Lookahead คือ เลือกข้อมูลข้างหน้า… ในโค้ดคือ `(?=")` เลือกข้อมูลหลัง `"`

สรุป จาก tag `a` จะคัดเลือกข้อมูลให้เหลือเพียงข้างใน `title=”____”`

#### 4. `/รายชื่อวัดใน/` (crawler/crawler.py บรรทัดที่ 130)

ใช้สำหรับค้นหาคำว่า `“รายชื่อวัดใน”` ใน attribute title เพื่อตรวจสอบว่าเป็น link สำหรับรายชื่อวัดในแต่ละจังหวัดหรือไม่

แกะ regex :

- โดย regex นี้จะเข้าเงื่อนไขก็ต่อเมื่อใน string มี `“รายชื่อวัดใน”` เนื่องจากใน https://th.wikipedia.org/wiki/หมวดหมู่:รายชื่อวัดไทย

เราต้องการส่วนที่จะดึงจังหวัดทุกจังหวัดมา และหาตาม parameter ที่ front end ได้ติ้กเลือกไว้

#### 5. `/(?<=title=")วัด.*?(?="|\s\()/` (crawler/crawler.py บรรทัดที่ 146)

ใช้ในการดึงชื่อวัดโดยจะค้นหาตัวอักษร `วัด` ที่อยู่ใน attribute title ของ anchor tag

แกะ regex:

- `(?<=...)` Positive Lookbehind เลือกข้อมูลข้างหลัง… ในโค้ดคือ (?<=title=") เลือกข้อมูลหลังคำว่า title=”\

- `.` Meta Escape คือ รับทุกตัวยกเว้น newline

- `*` Repetition Operator ในโค้ดคือ .\* เพื่อรับอักขระตัวใดก็ได้ ยกเว้น newline(“\n”) เป็นจำนวน 0 ถึง n ตัว

- `*?` Non-greedy Quantifier ในโค้ดคือ .\*? เพื่อรับสตริงที่ตรงเงื่อนไข ที่สั้นที่สุด

- `(?=...)` Positive Lookahead คือ เลือกข้อมูลข้างหน้า… ในโค้ดคือ (?="|\s\() เลือกข้อมูลหลัง " หรือ (

- `\s` หมายถึง Whitespace

- `\(` หมายถึง `( ( \` ทำหน้าที่เป็น Escape Character)

สรุป จาก tag `<a>` ในหน้ารายชื่อวัดของแต่ละจังหวัด จะดึงข้อมูลให้เหลือเพียงชื่อวัดที่อยู่ระหว่าง `title=”` และ `“` หรือ `(` เท่านั้น