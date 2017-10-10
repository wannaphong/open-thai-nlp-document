
# โปรแกรมเช็คคำไทยแท้

พัฒนาโดย นาย วรรณพงษ์ ภัททิยไพบูลย์

เขียนเมื่อ 10 ตุลาคม พ.ศ.2560

[![License: CC BY-NC-SA 4.0](https://img.shields.io/badge/License-CC%20BY--NC--SA%204.0-lightgrey.svg)](https://creativecommons.org/licenses/by-nc-sa/4.0/)


```python
import re
```


```python
def check1(word): # เช็คตัวสะกดว่าตรงตามมาตราไหม
	if word in ['ก','ด','บ','น','ง','ม','ย','ว']:
		return True
	else:
		return False
```

ส่วน check1 ใช้การตรวจสอบโดยต้องดึงพยัญชนะสุดท้ายของคำ คือ ตัวสะกด ให้ตัวสะกดนั้นเป็น ก ด บ น ง ม ย ว หรือไม่ ถ้าเป็น แสดงว่าเป็นคำไทยแท้

อ้างอิง ข้อ 2 จาก 
[ลักษณะของคำไทยแท้](http://www.trueplookpanya.com/learning/detail/30589-043067)


```python
def check2(word): # เช็คตัวการันต์ ถ้ามี ไม่ใช่คำไทยแท้
	if '์' in word:
		return False
	else:
		return True
```


```python
def check3(word):
	if word in list("ฆณฌฎฏฐฑฒธศษฬ"): # ถ้ามี แสดงว่าไม่ใช่คำไทยแท้
		return False
	else:
		return True
```


```python
def thaicheck(word):
	pattern = re.compile(r"[ก-ฬฮ]",re.U) # สำหรับตรวจสอบพยัญชนะ
	res = re.findall(pattern,word) # ดึงพยัญชนะทัั้งหมดออกมา
	if res==[]:
		return False
	elif check1(res[len(res)-1]) or len(res)==1:
		if check2(word):
			word2=list(word)
			i=0
			thai=True
			if word in ['ฆ่า','เฆี่ยน','ศึก','ศอก','เศิก','เศร้า','ธ','ณ','ฯพณฯ','ใหญ่','หญ้า','ควาย','ความ','กริ่งเกรง','ผลิ']: # ข้อยกเว้น คำเหล่านี้เป็นคำไทยแท้
				return True
			while i<len(word2) and thai==True:
				thai= check3(word2[i])
				i+=1
			return True
		else:
			return False
	elif word in ['กะ','กระ','ปะ','ประ']:
		return True
	else:
		return False
```


```python
from pythainlp.tokenize.pyicu import segment # ใช้ในการตัดคำ
# สาเหตุที่เลือก icu เนื่องจากตัดคำไทยได้ค่อนข้างแน่นอนกว่าวิธีอื่น
```


```python
def thai(word):
	words=segment(word)
	one_word=len(words)==1
	if one_word:
		return [(word,thaicheck(word))]
	else:
		i=0
		m=[]
		while i < len(words):
			m.append((words[i],thaicheck(words[i])))
			i+=1
		return m
```


```python
print(thai("ตา"))
print(thai("คน"))
print(thai("มอ"))
print(thai("บุคคล")) # ภาษาบาลี
print(thai("เลข")) # ภาษาบาลี
print(thai("วัดกับวาด"))
```

    [('ตา', True)]
    [('คน', True)]
    [('มอ', True)]
    [('บุคคล', False)]
    [('เลข', False)]
    [('วัด', True), ('กับ', True), ('วาด', True)]


**ปัญหาที่พบ**

- ใช้งานได้เฉพาะที่เขียนอย่างไทย ทำให้คำยืมที่เขียนอย่างไทย โปรแกรมคืนค่าออกมาเป็นคำไทยแท้
- ใช้งานได้เฉพาะคำทีละ 1 คำ

**อ้างอิง**

ทีมงานทรูปลูกปัญญา.ลักษณะของคำไทยแท้.2015.(ออนไลน์).แหล่งที่มา : http://www.trueplookpanya.com/learning/detail/30589-043067. 10 ตุลาคม 2017

วารุณี บำรุงรส.คำไทยแท้.2010.(ออนไลน์).แหล่งที่มา : https://www.gotoknow.org/posts/377619. 10 ตุลาคม 2017