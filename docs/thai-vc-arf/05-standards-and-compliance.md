---
description: "5. มาตรฐานและข้อปฏิบัติสำหรับเอกสารรับรองดิจิทัลและเอกสารสำแดงดิจิทัล (Standards and Compliance for Verifiable Credentials and Presentations) — Thai VC ARF 2.0 DRAFT 0"
---

# 5. มาตรฐานและข้อปฏิบัติสำหรับเอกสารรับรองดิจิทัลและเอกสารสำแดงดิจิทัล (Standards and Compliance for Verifiable Credentials and Presentations)

{/* METADATA (agent-only — not rendered to readers)
> **สถานะ:** 📐 Specification — อธิบายโปรไฟล์ SD-JWT VC ของเวอร์ชัน 2.0 โดยยังรองรับ JSON-LD VC รูปแบบเดิม
> **เวอร์ชันเอกสาร:** 2.0 (10 สิงหาคม 2569) — ปรับปรุงตัวอย่างเป็น SD-JWT VC และโพรโทคอล OID4VCI/OID4VP 1.0 Final (MASA-170)
> **แหล่งข้อมูลเดิม:** [Thai-VC-ARF-v1-1.pdf](https://www.etda.or.th/getattachment/Our-Service/Digital-Trusted-services-Infrastructure/VC-and-Digital-Document-Wallet/Information/รายงานทางเทคนค-Thai-VC-ARF-v1-1.pdf) — รายงานทางเทคนิค: กรอบแนวทางการทำงานร่วมกันของเอกสารรับรองดิจิทัลสำหรับประเทศไทย (มกราคม 2568) — บทนี้ปรับปรุงจากต้นฉบับดังกล่าว รายละเอียดที่ถูกแทนที่ดูได้จาก git history ของไฟล์นี้
> **เอกสารที่เกี่ยวข้อง:** [ดัชนีเอกสาร ARF](README.md) · [§8 — Implementation Guidelines](08-implementation-guidelines.md) · [§8.1 — OID4VCI Full Flow](08.1-issuance-full-flow-detail.md) · [§8.2 — OID4VP Full Flow](08.2-oid4vp-full-flow-detail.md) · [§11 — Cryptographic Suites](11-cryptographic-suites.md) · [ภาคผนวก ข — บันทึกการเปลี่ยนแปลง v1.1→v2.0](15-appendix.md)
*/}

---

{/* METADATA (agent-only — not rendered to readers)
> **ขอบเขตการปรับปรุง:** รายงานเวอร์ชัน 1.1 ใช้ JSON-LD VC ตาม W3C VC Data Model ซึ่งยังรองรับอยู่ บทนี้อธิบายตัวอย่างโปรไฟล์ SD-JWT VC (`dc+sd-jwt`) ของเวอร์ชัน 2.0 พร้อม OID4VCI 1.0 Final และ OID4VP 1.0 Final (DCQL) การเพิ่มโปรไฟล์นี้ไม่ได้ยกเลิกการรองรับ JSON-LD VC เดิม ดูบันทึกการเปลี่ยนแปลงใน [ภาคผนวก ข](15-appendix.md)
*/}

เมื่อการทำธุรกรรมมีแนวโน้มมาอยู่ในรูปแบบอิเล็กทรอนิกส์มากขึ้น หน่วยงานทั้งภาครัฐและภาคเอกชนอาจมีการออกเอกสารรับรองข้อมูลในรูปของเอกสารรับรองดิจิทัลที่ตรวจสอบได้ (verifiable credential: VC) โดยเอกสารรับรองดิจิทัลที่ตรวจสอบได้ดังกล่าวเป็นเอกสารรับรองข้อมูลในรูปแบบอิเล็กทรอนิกส์ที่มีคุณสมบัติช่วยให้สามารถตรวจพบการปลอมแปลงและตรวจสอบผู้เขียนข้อมูลได้ด้วยกระบวนการเข้ารหัสลับ (Cryptography)

ในรายงานฉบับนี้ เอกสารรับรองข้อมูลตามที่กล่าวข้างต้นประกอบด้วย 2 ส่วน ดังนี้:

- เอกสารรับรองดิจิทัล (verifiable credential: VC)
- เอกสารสำแดงดิจิทัล (verifiable presentation: VP)

ระบบยังรองรับ JSON-LD VC ตาม W3C VC Data Model v1.1 [5] ส่วนตัวอย่างในบทนี้แสดงโปรไฟล์ SD-JWT VC (`dc+sd-jwt`) ของเวอร์ชัน 2.0

ตัวอย่างในบทนี้ใช้ **IETF SD-JWT Verifiable Credential (`dc+sd-jwt`)** [12] ส่งผ่านด้วยโพรโทคอล **OID4VCI 1.0 Final** [10] สำหรับการออกเอกสาร และ **OID4VP 1.0 Final พร้อม DCQL** [11] สำหรับการสำแดงเอกสาร โดยระบบยังรองรับ JSON-LD VC รูปแบบเดิม รายละเอียดของโปรไฟล์ SD-JWT อยู่ใน §5.1–§5.4 ด้านล่าง ส่วนรายละเอียดทางเทคนิคระดับ field-by-field อยู่ที่ [§8.1 — OID4VCI Full Flow](08.1-issuance-full-flow-detail.md) และ [§8.2 — OID4VP Full Flow](08.2-oid4vp-full-flow-detail.md)

## 5.0 โครงของบทนี้

บทนี้มีเนื้อหายาว รายงานฉบับนี้จึงแบ่งออกเป็น 5 ส่วนหลัก เพื่อให้ผู้อ่านเข้าถึงหัวข้อที่ต้องการได้สะดวก ดังนี้

| ส่วน | หัวข้อในบท | เนื้อหาโดยย่อ |
|------|-----------|--------------|
| โครงสร้าง VC/VP | §5.1–§5.2 | องค์ประกอบของเอกสารรับรองดิจิทัล (VC) และเอกสารสำแดงดิจิทัล (VP) ในรูปแบบ **SD-JWT VC (`dc+sd-jwt`)** พร้อมตัวอย่าง |
| การออกเอกสาร (OID4VCI) | §5.3 | ขั้นตอนการออก VC ด้วยโพรโทคอล **OID4VCI 1.0 Final** [10] |
| การสำแดงเอกสาร (OID4VP) | §5.4 | ขั้นตอนการสำแดงและตรวจสอบ VP ด้วยโพรโทคอล **OID4VP 1.0 Final พร้อม DCQL** [11] |
| Universal Resolver | §5.5 | การใช้ DIF Universal Resolver เพื่อการทำงานร่วมกันระหว่างระบบนิเวศ |
| บทบาทของผู้เกี่ยวข้อง | §5.6 | บทบาทของผู้ออกเอกสาร ผู้ถือเอกสาร และผู้ตรวจสอบเอกสาร |

## 5.1 เอกสารรับรองดิจิทัล (verifiable credential: VC)

เอกสารรับรองดิจิทัล ประกอบด้วย identifier และคำอธิบายข้อมูล (metadata) เช่น ผู้ออกเอกสาร วันและเวลาเมื่อ VC เริ่มมีผลผูกพันและสิ้นผลผูกพัน

โครงสร้างของ SD-JWT VC ในตัวอย่างนี้แสดงตามรูปที่ 4 ซึ่งประกอบด้วย 3 ส่วน ได้แก่ (1) คำอธิบายข้อมูลของ VC (credential metadata) (2) ข้อความยืนยัน (claim) และ (3) ข้อพิสูจน์ (proof) โดยบันทึกตามเค้าร่าง IETF SD-JWT Verifiable Credential (`dc+sd-jwt`) [12] ส่วน JSON-LD VC ตาม W3C VC Data Model v1.1 [5] ยังคงรองรับเป็นรูปแบบเดิม

![โครงสร้างเอกสารรับรอง (VC)](images/p15-0.png)

**รูปที่ 4 โครงสร้างเอกสารรับรอง (VC)**

ตัวอย่างเอกสารรับรองต่อไปนี้ใช้เค้าร่าง **IETF SD-JWT Verifiable Credential (`dc+sd-jwt`)** [12] ซึ่งบันทึกทั้ง 3 ส่วนข้างต้นในรูปแบบ SD-JWT โดย SD-JWT VC ประกอบด้วย 3 ส่วนเรียงต่อกันด้วยสัญลักษณ์ "~" ได้แก่

1. **Issuer-signed JWT** — JOSE header ระบุประเภทเอกสาร (`typ: "dc+sd-jwt"`) และวิธีลงลายมือชื่อ (`alg`) ตามด้วย JWT claims ที่มี claim ซึ่งถูกแฮชไว้ในอาร์เรย์ `_sd` (แทนการเปิดเผยค่าจริงในตัวเอกสาร) พร้อมกุญแจสาธารณะของผู้ถือเอกสาร (`cnf.jwk`) สำหรับผูกเอกสารไว้กับผู้ถือ
2. **Disclosures** — รายการค่าจริงของแต่ละ claim ที่เข้ารหัส Base64URL แยกทีละรายการ ผู้ถือเอกสารเลือกเปิดเผยเฉพาะรายการที่ต้องการได้ในภายหลัง โดยไม่กระทบลายมือชื่อของผู้ออกเอกสาร
3. **Key Binding JWT (KB-JWT)** — JWT ที่ผู้ถือเอกสารลงลายมือชื่อด้วยกุญแจของตนเอง (คู่กับ `cnf.jwk` ในข้อ 1) เพื่อพิสูจน์การครอบครองคู่กุญแจ สร้างขึ้นเฉพาะตอนนำเอกสารไปสำแดง (ดู §5.2)

ในรายงานฉบับนี้ ได้ยกตัวอย่างกรณีศึกษาของ VC ที่แสดงกรณีศึกษาเรื่องการขอใบประมวลผลการศึกษา (Transcript) ตามรูปที่ 6 จากมหาวิทยาลัย โดยเอกสารจะออกให้เมื่อนักศึกษาพิสูจน์ได้ว่าตนเองเคยศึกษา ณ มหาวิทยาลัยดังกล่าว ซึ่งทางมหาวิทยาลัยได้ออก VC เพื่อรับรองการสำเร็จการศึกษา และนักศึกษาจัดเก็บ VC นั้นไว้ในกระเป๋าเอกสารดิจิทัลของตนเอง

![VC JWT Format](images/p15-1.png)

**รูปที่ 5 VC JWT Format**

รูปที่ 5 แสดงแนวคิดทั่วไปของการบันทึกข้อมูล VC เป็นโครงสร้างหลายส่วนที่ลงลายมือชื่อแบบ JOSE ซึ่งเป็นแนวคิดเดียวกับที่ SD-JWT VC นำมาใช้ ต่างกันเพียงตัวคั่นส่วนย่อยเป็น "~" แทน "." และมีส่วน Disclosures กับ KB-JWT เพิ่มเข้ามาเพื่อรองรับ selective disclosure ตามที่อธิบายไว้ข้างต้น

![กรณีศึกษา VC การขอใบประมวลผลการศึกษา (Transcript)](images/p16-0.png)

**รูปที่ 6 กรณีศึกษา VC การขอใบประมวลผลการศึกษา (Transcript)**

ขั้นตอนการออกใบประมวลผลการศึกษามีกระบวนการ (User Journey) ดังนี้

1. นักศึกษาหรือผู้ที่ต้องการหลักฐานการศึกษาส่งคำร้อง (request) ให้กับทางมหาวิทยาลัย ผ่านช่องทางที่เป็น web application และมีการยืนยันตัวตน (user authentication)
2. เมื่อนักศึกษาร้องขอเอกสารใบประมวลผลการศึกษา (Transcript) ไปยังมหาวิทยาลัย จากนั้นมหาวิทยาลัยจะดำเนินการสร้างคิวอาร์โคด (QR code) สำหรับรายละเอียดการเชื่อมต่อกับ wallet รับเอกสาร นักศึกษาสแกนคิวอาร์โคด (QR code) เพื่อขอรับเอกสาร และมหาวิทยาลัยจัดเตรียมข้อมูลสำหรับใบประมวลผลการศึกษา (Transcript) ในรูปแบบเอกสารรับรองดิจิทัล (VC) และทำการลงลายมือชื่ออิเล็กทรอนิกส์
3. เมื่อนักศึกษาได้รับใบประมวลผลการศึกษา (Transcript) ในรูปแบบเอกสารรับรองดิจิทัล (VC) แล้วจึงนำเอกสารที่ได้รับไปจัดเก็บในกระเป๋าเอกสารดิจิทัลของนักศึกษา (digital document wallet)
4. เมื่อนักศึกษาต้องการไปสมัครงานกับบริษัทสมมติเป็นผู้ตรวจสอบเอกสาร ทางบริษัทจะส่งคำร้องขอ VC ที่ออกโดยมหาวิทยาลัยมายังกระเป๋าเอกสารดิจิทัลของนักศึกษา และกระเป๋าเอกสารดิจิทัลจะถามนักศึกษาว่าต้องการใช้ VC ที่มีอยู่หรือไม่ เมื่อนักศึกษาตอบตกลง VC จะถูกนำมาใช้สร้างเป็นเอกสารสำแดงดิจิทัล (VP) และส่งต่อไปให้บริษัทดำเนินการตรวจสอบ

**ตัวอย่างที่ 1 โครงสร้าง VC ที่ใช้ในโครงการ แสดงรูปแบบ SD-JWT VC (`dc+sd-jwt`)**

JOSE Header (ส่วนที่ 1 ของ Issuer-signed JWT):

```json
{
  "typ": "dc+sd-jwt",
  "alg": "EdDSA",
  "kid": "https://credential-issuer.example.com#key-1"
}
```

JWT Claims (ส่วนที่ 2 ของ Issuer-signed JWT):

```json
{
  "iss": "https://credential-issuer.example.com",
  "vct": "TranscriptCredential",
  "iat": 1755500991,
  "exp": 1755501291,
  "cnf": {
    "jwk": { "kty": "OKP", "crv": "Ed25519", "x": "holder_pub_key_x" }
  },
  "_sd_alg": "sha-256",
  "_sd": [
    "TGZlNVR4Rk1md0dhUUdicjFXODNZa3VqVWJPYld4Wko3ZTgxU1RCVVo1SQ",
    "OFdVaXVUb2R2TzFyaWt6TzZrb0J0dUlvMEZuVjV5cE5xUUNMSDJZeXNXVQ"
  ]
}
```

Disclosure (ส่วนที่ 2 ของ SD-JWT VC — ตัวอย่างการเปิดเผย claim `student`):

```
["3jqcb67z9wksrndv", "student", "Nattapong Methawon"]
```

| ส่วน | ตัวคั่น | เนื้อหา |
|------|--------|--------|
| Issuer-signed JWT | `~` หลังส่วนนี้ | JOSE header + JWT claims ที่ลงลายมือชื่อโดยมหาวิทยาลัย (ผู้ออกเอกสาร) |
| Disclosures | `~` คั่นระหว่างแต่ละรายการ | ค่าจริงของแต่ละ claim (เช่น `student`, `gpa`) ที่เข้ารหัส Base64URL แยกทีละรายการ |
| Key Binding JWT | ต่อท้ายสุด | สร้างเฉพาะตอนนำไปสำแดงเป็น VP (ดู §5.2) — ไม่มีในตัว VC ที่จัดเก็บไว้ในกระเป๋าเอกสาร |

รายละเอียดโครงสร้าง field-by-field พร้อมตัวอย่างที่ใช้งานจริงในระบบอยู่ที่หัวข้อ STEP 6 — Issue VC ใน [§8.1 — OID4VCI Full Flow](08.1-issuance-full-flow-detail.md) และหมวดอัลกอริทึมที่รองรับอยู่ที่ [§11 — Cryptographic Suites](11-cryptographic-suites.md)

## 5.2 เอกสารสำแดงดิจิทัล (verifiable presentation: VP)

เอกสารสำแดงข้อมูลที่ประกอบด้วย VC อย่างน้อยหนึ่งชุด ตามรูปที่ 4 ซึ่งอาจเป็นข้อมูลต้นฉบับตามที่ปรากฏใน VC หรือเป็นข้อมูลที่สังเคราะห์จาก VC ก็ได้ โดย VP จะมีคุณสมบัติที่สามารถตรวจพบการเปลี่ยนแปลงใด ๆ ที่เกิดกับความถูกต้องครบถ้วนของข้อมูล และตรวจสอบลายมือชื่ออิเล็กทรอนิกส์ของผู้ถือเอกสารและตรวจสอบ VC ที่เกี่ยวข้องได้ด้วยกระบวนการเข้ารหัสลับ

การสังเคราะห์ข้อมูลจาก VC เพื่อสร้างเป็น VP ใช้วิธีการที่รองรับการเลือกเปิดเผยข้อมูลบางส่วน (selective disclosure) ตามที่อธิบายไว้ใน §5.1 กล่าวคือ ผู้ถือเอกสารเลือกส่งเฉพาะ Disclosure ของ claim ที่ต้องการเปิดเผย โดยไม่ต้องส่ง claim อื่นที่อยู่ใน VC ทั้งหมด เช่น เลือกแสดงเฉพาะชื่อและนามสกุลจากข้อมูลทั้งหมดตามหน้าบัตรประชาชน โดยไม่เปิดเผยข้อมูลส่วนอื่น

โครงสร้างการสำแดง SD-JWT VC ในตัวอย่างนี้แสดงตามรูปที่ 7 ซึ่งประกอบด้วย 3 ส่วน ได้แก่ (1) คำอธิบายข้อมูลของ VP (presentation metadata) (2) เอกสารรับรองดิจิทัล (VC) และ (3) ข้อพิสูจน์ (proof) ส่วนข้อพิสูจน์คือ Key Binding JWT (KB-JWT) ที่ผู้ถือเอกสารสร้างขึ้นและลงลายมือชื่อด้วยกุญแจของตนเอง — SD-JWT VC ไม่ต้องมี JWT-VP ห่ออีกชั้น เพราะ KB-JWT ทำหน้าที่พิสูจน์การครอบครองเอกสารในตัวอยู่แล้ว

สำหรับในรายงานฉบับนี้ การตรวจสอบเอกสารสำแดงดิจิทัลมีขั้นตอนการตรวจสอบดังนี้

1. ตรวจสอบความถูกต้องของโครงสร้างเอกสารสำแดงดิจิทัล โดยแยกส่วนด้วยสัญลักษณ์ "~" แล้วตรวจว่าประกอบด้วย Issuer-signed JWT, Disclosures และ Key Binding JWT ครบถ้วนตามรูปแบบ SD-JWT
2. ตรวจสอบ Key Binding JWT (KB-JWT) โดยตรวจลายมือชื่อด้วยกุญแจสาธารณะของผู้ถือเอกสาร (`cnf.jwk`) ที่ปรากฏใน Issuer-signed JWT และตรวจว่าค่า `sd_hash` ใน KB-JWT ตรงกับค่าแฮชที่คำนวณจากส่วน Issuer-signed JWT และ Disclosures ที่ส่งมาจริง เพื่อยืนยันว่าผู้ส่งเป็นผู้ครอบครองคู่กุญแจของเอกสารจริง
3. ตรวจสอบผู้ออกเอกสาร (Issuer) เป็น Legal Entity หรือไม่ โดยการร้องขอ DID Document จาก Registry ผ่าน DIF Universal Resolver
4. ตรวจสอบ Disclosure แต่ละรายการที่เปิดเผยมาว่าเมื่อคำนวณแฮชแล้วตรงกับค่าที่ปรากฏในอาร์เรย์ `_sd` ของ Issuer-signed JWT หรือไม่ เพื่อยืนยันว่าข้อมูลที่เปิดเผยไม่ถูกแก้ไข
5. ตรวจสอบความถูกต้องของ Issuer-signed JWT ว่าได้ออกมาอย่างถูกต้องผ่าน Issuer นั้นจริง โดยตรวจลายมือชื่อดิจิทัลของผู้ออกเอกสารด้วยกุญแจสาธารณะที่ได้จาก DID Document หรือ Trusted List

เมื่อทำการตรวจสอบทั้ง 5 ขั้นตอนแล้วถูกต้องทั้งหมด จึงจะยอมรับว่า VP ที่ได้รับมาถูกสร้างออกมาอย่างถูกต้อง จึงจะนำไปใช้งานหรือดำเนินการอื่นต่อไปได้ รายละเอียดขั้นตอนการตรวจสอบเต็มรูปแบบ (รวม Trust Checkpoint ของ ETDA Trust Model) อยู่ที่ [§8.2 — OID4VP Full Flow](08.2-oid4vp-full-flow-detail.md)

![โครงสร้างเอกสารสำแดงดิจิทัล VP](images/p19-0.png)

**รูปที่ 7 โครงสร้างเอกสารสำแดงดิจิทัล VP**

จากกรณีศึกษาในตัวอย่างที่ 1 เมื่อนักศึกษาได้รับ VC มาจัดเก็บไว้ในกระเป๋าเอกสารดิจิทัลของตนเองแล้ว ต่อมาจะนำเอกสารไปทำการสมัครงานจากบริษัทสมมติซึ่งเป็นผู้ตรวจสอบเอกสาร บริษัทที่รับสมัครจะส่งคำร้องขอ VC ที่ออกโดยมหาวิทยาลัยผ่านไปยังกระเป๋าเอกสารดิจิทัลของนักศึกษา และกระเป๋าเอกสารดิจิทัลจะถามนักศึกษาว่าต้องการใช้ VC ที่มีอยู่หรือไม่ เมื่อนักศึกษาตอบตกลง VC จะถูกนำมาใช้สร้างเป็น VP แล้วส่งต่อไปให้บริษัททำการตรวจสอบ ตัวอย่างของ VP ข้างต้นในรูปแบบ SD-JWT Presentation เป็นดังนี้

**ตัวอย่างที่ 2 โครงสร้าง VP ที่ใช้ในโครงการ แสดงรูปแบบ SD-JWT Presentation**

Key Binding JWT (KB-JWT) — ส่วนสุดท้ายของ SD-JWT Presentation:

```json
{
  "typ": "kb+jwt",
  "alg": "ES256"
}
.
{
  "iss": "wallet-instance-abc123",
  "aud": "https://verifier.example.com",
  "nonce": "fa2450d7-8ca5-40d2-8832-b03891c355be",
  "iat": 1755501000,
  "sd_hash": "9GzX9RVLtj7dCbCyeGnHb-8O0R5Fp2VW8LTkfWzePUM"
}
```

**การประกอบ VP ฉบับสมบูรณ์:** `<Issuer-signed JWT>~<Disclosure ของ student>~<Disclosure ของ gpa>~<KB-JWT>`

จากตัวอย่างที่ 1 และ 2 องค์ประกอบของ SD-JWT VC และ VP แบ่งออกเป็นส่วนหลัก ดังนี้

1. **JOSE header** (`typ`, `alg`, `kid`) ระบุประเภทเอกสาร ประเภทลายมือชื่อ และตัวชี้กุญแจที่ใช้ตรวจสอบ
2. **`iss`** เป็น claim ที่ระบุตัวตนของผู้ออกเอกสาร (URI แบบ https)
3. **`vct`** (Verifiable Credential Type) เป็น claim ที่ระบุประเภทของเอกสารรับรองตามมาตรฐาน IETF SD-JWT VC [12] ทำหน้าที่แทนคุณสมบัติ `type` ที่รายงานต้นฉบับเวอร์ชัน 1.1 เคยใช้ตามโครงสร้าง W3C VC Data Model v1.1 [5] ซึ่งไม่ใช้แล้วในเวอร์ชัน 2.0 นี้
4. **`iat`/`exp`** เป็น claim ที่ระบุวันและเวลาที่ออกเอกสาร (issuance) และวันหมดอายุ (expiration) ตามลำดับ
5. **`cnf.jwk`** เป็น claim ที่ระบุกุญแจสาธารณะของผู้ถือเอกสาร ใช้ผูกเอกสารไว้กับผู้ถือ (holder binding) และใช้ตรวจสอบ Key Binding JWT ตอนสำแดงเอกสาร
6. **`_sd` และ Disclosures** คือกลไก selective disclosure — `_sd` เก็บค่าแฮชของแต่ละ claim ส่วน Disclosure เก็บค่าจริงที่เปิดเผยได้ทีละรายการ แทนคุณสมบัติ `credentialSubject` ในโครงสร้าง JSON-LD
7. **Key Binding JWT (KB-JWT)** เป็นข้อพิสูจน์ (proof) ที่ผู้ถือเอกสารลงลายมือชื่อด้วยกุญแจของตนเอง ทำหน้าที่แทน `proof` ของผู้ถือเอกสารในโครงสร้าง JSON-LD และยืนยันว่าเป็นผู้ครอบครองเอกสารรับรองจริง

รายละเอียดขั้นตอนสร้างและตรวจ KB-JWT พร้อมตัวอย่างที่ใช้งานจริงอยู่ที่ [§8.2 — OID4VP Full Flow](08.2-oid4vp-full-flow-detail.md)

## 5.3 Protocol สำหรับการออก VC

โพรโทคอล **OpenID for Verifiable Credential Issuance (OID4VCI) 1.0 Final** [10] ใช้สำหรับการออกเอกสารรับรองดิจิทัลผ่าน API และรองรับเอกสารได้หลายรูปแบบ ตัวอย่างในบทนี้ใช้ **IETF SD-JWT Verifiable Credential (`dc+sd-jwt`)** [12] ตาม §5.1 และระบุ `format: "dc+sd-jwt"` ในคำขอของตัวอย่าง การรองรับ JSON-LD VC รูปแบบเดิมยังคงอยู่

![การใช้งาน OID4VC](images/p22-0.png)

**รูปที่ 8 การใช้งาน OID4VC**

![ขั้นตอนการทำ OID4VCI](images/p22-1.png)

**รูปที่ 9 ขั้นตอนการทำ OID4VCI**

### 5.3.1 กระบวนการทำงาน OID4VCI

การออกเอกสารรับรองดิจิทัล (VC) ของใบประมวลผลการศึกษา ใช้โพรโทคอล OID4VCI 1.0 Final ผ่าน Pre-Authorized Code Flow มีขั้นตอนการทำงานตามรูปที่ 9 ดังนี้

1. การให้ความยินยอมและการรวบรวมข้อมูล
2. การสร้าง Credential Offer
3. การดึงข้อมูล Metadata ผู้ออกเอกสาร
4. การร้องขอ Token
5. การร้องขอและการออก Credential Request

#### (1) การให้ความยินยอมและการรวบรวมข้อมูล

ผู้ออกเอกสาร (Credential Issuer) ต้องได้รับการยินยอมจากผู้ถือเอกสาร และรวบรวมข้อมูลที่จำเป็นของผู้ถือเอกสารสำหรับการออกเอกสารรับรองดิจิทัลผ่านกระบวนการเฉพาะ เช่น การยืนยันตัวตน (Authentication) และการอนุญาต (Authorization) ของผู้ถือเอกสาร โดยกระบวนการนี้ไม่อยู่ในขอบเขตของ OID4VCI

#### (2) การสร้าง Credential Offer

ผู้ออกเอกสารสร้าง Credential Offer สำหรับเอกสารรับรองที่จะทำการออกและส่งไปยังกระเป๋าเอกสารดิจิทัล โดยมีวิธีส่งได้หลายวิธี แต่ในการศึกษาครั้งนี้ผู้ออกเอกสารจะสร้าง QR Code ให้กระเป๋าเอกสารสแกน หรือคัดลอก URI ส่งให้ผ่านช่องทางอื่น Credential Offer ประกอบด้วย URL ของผู้ออกเอกสาร, ตัวระบุ Credential Configuration และ Pre-Authorized Code

**ตัวอย่าง credential offer**

```json
{
    "credential_issuer": "https://credential-issuer.example.com",
    "credential_configuration_ids": ["TranscriptCredential"],
    "grants": {
        "urn:ietf:params:oauth:grant-type:pre-authorized_code": {
            "pre-authorized_code": "sX2CpoKx",
            "tx_code": {
                "length": 4,
                "input_mode": "numeric",
                "description": "Please provide the one-time code that was sent via e-mail"
            }
        }
    }
}
```

**คำอธิบายพารามิเตอร์ (Pre-Authorized Code)**

| พารามิเตอร์ | Required | Type | รายละเอียด |
|-------------|----------|------|------------|
| credential_issuer | Required | URL | URL ของผู้ออกเอกสารที่คำนึงถึงตัวพิมพ์เล็ก-ใหญ่ ใช้โปรโตคอล https และต้องประกอบด้วย scheme และ host รวมถึง port number และ path components ได้ตามต้องการ แต่ต้องไม่มีส่วนประกอบ query หรือ fragment |
| credential_configuration_ids | Required | Array | อาร์เรย์ของข้อความที่ไม่ซ้ำกัน โดยแต่ละข้อความคือตัวระบุ Credential Configuration ที่ปรากฏใน `credential_configurations_supported` ของ Metadata ผู้ออกเอกสาร ซึ่งผู้ออกเอกสารสามารถออกให้ได้สำหรับ Credential Offer ครั้งนี้ |
| grants | Optional | Grant Type | ระบุ Grant Types ที่ Authorization Server ของผู้ออกเอกสารใช้สำหรับ Credential Offer ครั้งนี้ โดยในการศึกษาใช้เป็น urn:ietf:params:oauth:grant-type:pre-authorized_code |
| pre-authorized_code | Required | String | รหัสที่แสดงถึงการอนุญาตของผู้ออกเอกสารให้ผู้ถือเอกสารใช้ขอรับเอกสารรับรองประเภทใดประเภทหนึ่ง รหัสนี้ต้องมีอายุสั้นและใช้ได้เพียงครั้งเดียว หากเลือกใช้ Pre-Authorized Code Flow ต้องมีพารามิเตอร์นี้ |
| tx_code | Optional | Object | ใช้ระบุว่า Authorization Server ต้องการ Transaction Code จากผู้ถือเอกสารพร้อมกับ Token Request ใน Pre-Authorized Code Flow หรือไม่ หาก Authorization Server ไม่ต้องการ Transaction Code พารามิเตอร์นี้จะไม่ถูกใส่เข้ามา Transaction Code มีวัตถุประสงค์เพื่อเชื่อมโยง Pre-Authorized Code กับธุรกรรมที่เฉพาะเจาะจง เพื่อป้องกันการใช้รหัสซ้ำโดยผู้โจมตี เช่น การสแกน QR code โดยไม่ได้รับอนุญาต แนะนำให้ส่ง Transaction Code ผ่านช่องทางอื่น |
| input_mode | Optional | String | กำหนดชุดตัวอักษรที่สามารถป้อนข้อมูลได้ ค่าเลือกที่เป็นไปได้คือเฉพาะตัวเลขหรือเฉพาะตัวอักษร ค่าเริ่มต้นคือตัวเลข |
| length | Optional | Integer | กำหนดความยาวของ Transaction Code ช่วยให้กระเป๋าเอกสารแสดงหน้าจอสำหรับป้อนข้อมูลได้ดีขึ้นและช่วยเพิ่มประสบการณ์การใช้งานของผู้ใช้ |
| description | Optional | String | คำแนะนำแก่ผู้ถือกระเป๋าเอกสาร เกี่ยวกับวิธีการรับรหัสธุรกรรม เช่น การบอกว่ารหัสจะถูกส่งผ่านช่องทางการสื่อสารใด แนะนำให้แสดงคำอธิบายนี้ถัดจากหน้าจอป้อน Transaction Code เพื่อปรับปรุงประสบการณ์การใช้งาน ข้อความนี้ต้องไม่เกิน 300 ตัวอักษร และไม่รองรับการแปลหลายภาษา |

#### (3) การดึงข้อมูล Metadata ผู้ออกเอกสาร

เพื่อดึง Metadata กระเป๋าเอกสารจะต้องส่ง HTTP GET ไปที่ URL ของผู้ออกเอกสารต่อด้วย `/.well-known/openid-credential-issuer` เช่น `https://credential-issuer.example.com/.well-known/openid-credential-issuer` หาก URL ของผู้ออกเอกสารมี path component ( / ) ที่อยู่ท้ายสุด จะต้องถูกลบออกก่อนที่จะเพิ่ม `/.well-known/openid-credential-issuer`

Metadata จะเป็น JSON ประกอบด้วย ประเภทและรูปแบบของเอกสารรับรองทั้งหมดที่ผู้ออกเอกสารสามารถออกได้, Token Endpoint, Credential Endpoint และอื่น ๆ

**ตัวอย่าง Metadata**

```json
{
    "credential_issuer": "https://credential-issuer.example.com",
    "authorization_servers": [ "https://server.example.com" ],
    "credential_endpoint": "https://credential-issuer.example.com/credential",
    "deferred_credential_endpoint": "https://credential-issuer.example.com/deferred_credential",
    "notification_endpoint": "https://credential-issuer.example.com/notification",
    "credential_response_encryption": {
        "alg_values_supported": [
            "ECDH-ES"
        ],
        "enc_values_supported": [
            "A128GCM"
        ],
        "encryption_required": false
    },
    "display": [
        {
            "name": "Example University",
            "locale": "en-US"
        },
        {
            "name": "Example Université",
            "locale": "fr-FR"
        }
    ],
    "credential_configurations_supported": {
        "TranscriptCredential": {
            "format": "dc+sd-jwt",
            "vct": "TranscriptCredential",
            "scope": "TranscriptCredential",
            "cryptographic_binding_methods_supported": [
                "jwk"
            ],
            "credential_signing_alg_values_supported": [
                "EdDSA"
            ],
            "claims": {
                "given_name": {
                    "display": [
                        {
                            "name": "Given Name",
                            "locale": "en-US"
                        }
                    ]
                },
                "family_name": {
                    "display": [
                        {
                            "name": "Surname",
                            "locale": "en-US"
                        }
                    ]
                },
                "degree": {},
                "gpa": {
                    "display": [
                        {
                            "name": "GPA"
                        }
                    ]
                }
            },
            "proof_types_supported": {
                "jwt": {
                    "proof_signing_alg_values_supported": [
                        "ES256"
                    ]
                }
            },
            "display": [
                {
                    "name": "University Credential",
                    "locale": "en-US",
                    "logo": {
                        "url": "https://university.example.edu/public/logo.png",
                        "alt_text": "a square logo of a university"
                    },
                    "background_color": "#12107c",
                    "text_color": "#FFFFFF"
                }
            ]
        }
    }
}
```

**คำอธิบายค่าพารามิเตอร์**

| พารามิเตอร์ | Required | Type | รายละเอียด |
|-------------|----------|------|------------|
| credential_issuer | Required | URL | URL ของผู้ออกเอกสารที่คำนึงถึงตัวพิมพ์เล็ก-ใหญ่ ใช้โปรโตคอล https และต้องประกอบด้วย scheme และ host รวมถึง port number และ path components ได้ตามต้องการ แต่ต้องไม่มีส่วนประกอบ query หรือ fragment |
| authorization_servers | Optional | Array of strings | อาร์เรย์ของสตริง ซึ่งแต่ละสตริงเป็นตัวระบุของ OAuth 2.0 Authorization Server ที่ผู้ออกเอกสารอ้างอิงเพื่อใช้ในการอนุมัติ หากไม่มีการระบุพารามิเตอร์นี้ ผู้ออกเอกสารจะทำหน้าที่เป็น Authorization Server ด้วย โดยใช้ตัวระบุของผู้ออกเอกสารเพื่อดึงข้อมูล Metadata ของ Authorization Server |
| credential_endpoint | Required | URL | URL ของ Credential Endpoint ของผู้ออกเอกสาร URL นี้ต้องใช้โปรโตคอล HTTPS |
| deferred_credential_endpoint | Optional | URL | URL ของ Deferred Credential Endpoint ของผู้ออกเอกสาร URL นี้ต้องใช้โปรโตคอล HTTPS หากไม่มีการระบุพารามิเตอร์นี้ แสดงว่าผู้ออกเอกสารไม่รองรับ Deferred Credential Endpoint |
| notification_endpoint | Optional | URL | URL ของ Notification Endpoint ผู้ออกเอกสารต้องใช้โปรโตคอล HTTPS หากไม่มีการระบุพารามิเตอร์นี้ แสดงว่าผู้ออกเอกสารไม่รองรับ Notification Endpoint |
| credential_response_encryption | Optional | Object | แสดงข้อมูลว่าผู้ออกเอกสารรองรับการเข้ารหัส Credential Response เพิ่มเติมจาก TLS หรือไม่ |
| alg_values_supported | Required* | Array | เป็นอาร์เรย์ที่ประกอบด้วยรายการของอัลกอริทึมการเข้ารหัส JWE ([RFC7516]) ที่ได้รับการสนับสนุนโดย Credential Endpoint เพื่อเข้ารหัสข้อมูล Credential Response ในรูปแบบ JWT ([RFC7519]) ตามที่กำหนดไว้ใน [RFC7518] |
| enc_values_supported | Required* | Array | เป็นอาร์เรย์ที่ประกอบด้วยรายการของอัลกอริทึมการเข้ารหัส JWE ([RFC7516]) (ค่า enc) ที่ได้รับการสนับสนุนโดย Credential Endpoint เพื่อเข้ารหัสข้อมูล Credential Response ในรูปแบบ JWT ([RFC7519]) ตามที่กำหนดไว้ใน [RFC7518] |
| encryption_required | Required* | Boolean | เป็นค่าบูลีนที่ระบุว่าผู้ออกเอกสารต้องการการเข้ารหัสเพิ่มเติมนอกเหนือจาก TLS สำหรับ Credential Response หรือไม่ หากค่าเป็น true แสดงว่าผู้ออกเอกสารต้องการการเข้ารหัสสำหรับ Credential Response ทุกครั้ง และกระเป๋าเอกสารจะต้องให้คีย์การเข้ารหัสในคำขอ Credential Request หากค่าเป็น false กระเป๋าเอกสารอาจเลือกได้ว่าจะให้คีย์การเข้ารหัสหรือไม่ |
| display | Optional | Array of objects | อาร์เรย์ที่มีคุณสมบัติการแสดงผลของผู้ออกเอกสารในแต่ละภาษา |
| credential_configurations_supported | Required | Object | วัตถุที่อธิบายรายละเอียดของเอกสารรับรองที่ผู้ออกเอกสารรองรับ แต่ละรายการระบุ `format: "dc+sd-jwt"`, `vct`, และ `claims` ที่ผู้ออกเอกสารรองรับตามโครงสร้างที่อธิบายใน §5.1 |

> *ต้องใส่เมื่อมี credential_response_encryption

#### (4) การร้องขอ Token

![Token Exchange](images/p29-0.png)

**รูปที่ 10 Token Exchange**

- กระเป๋าเอกสารส่ง Pre-Authorized Code ที่ได้รับในขั้นตอนที่ 3 ไปยัง Token Endpoint หากผู้ออกเอกสารกำหนดให้ต้องใช้ Transaction Code กระเป๋าเอกสารจะส่งไปด้วย
- ผู้ออกเอกสารรับคำร้องขอโทเคนและทำการตรวจสอบสิทธิ์การเข้าถึงข้อมูล หากถูกต้องผู้ออกเอกสารจะดำเนินการสร้าง Access Token และส่ง Token กลับไปให้กระเป๋าเอกสาร

**ตัวอย่างการส่ง Token Request ของกระเป๋าเอกสาร (Pre-Authorized Code)**

```http
POST /token HTTP/1.1
Host: server.example.com
Content-Type: application/x-www-form-urlencoded

grant_type=urn:ietf:params:oauth:grant-type:pre-authorized_code
&pre-authorized_code=SplxlOBeZQQYbYS6WxSbIA
&tx_code=493536
```

**คำอธิบายค่าพารามิเตอร์ (Pre-Authorized Code)**

| พารามิเตอร์ | Required | Type | รายละเอียด |
|-------------|----------|------|------------|
| grant_type | Required | String | ระบุ Grant Types ที่ Authorization Server ของผู้ออกเอกสารใช้สำหรับ Credential Offer ครั้งนี้ โดยในการศึกษาใช้เป็น urn:ietf:params:oauth:grant-type:pre-authorized_code |
| pre-authorized_code | Required | String | รหัสที่ได้จากผู้ออกเอกสารที่แสดงถึงการอนุญาตเพื่อขอรับเอกสารรับรองประเภทใดประเภทหนึ่ง |
| tx_code | Optional | String | Transaction Code ที่ได้จากผู้ออกเอกสาร |

Issuer ดำเนินการสร้าง Access Token ตามรูปที่ 10 และส่ง Token กลับไปให้ Holder ระบุ parameter ดังนี้

**ตัวอย่างการส่ง HTTP Response ของผู้ออกเอกสาร (Pre-Authorized Code)**

```http
HTTP/1.1 200 OK
Content-Type: application/json

{
    "access_token": "eyJhbGciOiJSUzI1NiIsInR5cCI6Ikp..sHQ",
    "token_type": "bearer",
    "expires_in": 86400,
    "c_nonce": "tZignsnFbp",
    "c_nonce_expires_in": 86400
}
```

**คำอธิบายค่าพารามิเตอร์**

| พารามิเตอร์ | Required | Type | รายละเอียด |
|-------------|----------|------|------------|
| access_token | Required | String | เป็น token ที่ใช้ในการเข้าถึงบริการหรือข้อมูลจากผู้ให้บริการต่าง ๆ ที่เชื่อถือ access_token นั้น ออกให้โดย Authorization Server |
| token_type | Required | String | ชนิดของ Token |
| expires_in | Required | Number | อายุการใช้งานของ access_token มีระยะเวลาเป็นวินาทีนับจากเวลาของการเริ่มสร้าง access_token |
| c_nonce | Optional | String | ค่าประเภทสตริงที่มี nonce ใช้ในการสร้าง proof of possession สำหรับ key proof |
| c_nonce_expires_in | Optional | Number | จำนวนที่ระบุอายุของ c_nonce เป็นวินาที |

#### (5) การร้องขอและการออกเอกสารรับรอง

![Credential Request](images/p31-0.png)

**รูปที่ 11 Credential Request**

- กระเป๋าเอกสารส่ง Credential Request ไปยัง Credential Endpoint ของผู้ออกเอกสาร โดยแนบ Access Token และ Proof of Possession (ถ้ามี) ของคู่คีย์ที่ผู้ออกเอกสารจะผูกไว้กับเอกสารรับรองที่จะออก ดังรูปที่ 11
- ผู้ออกเอกสารตรวจสอบ Access Token และ Proof of Possession (ถ้ามี)
- เมื่อผ่านการตรวจสอบ ผู้ออกเอกสารจะส่งเอกสารรับรองกลับไปยังกระเป๋าเอกสารใน Credential Response

**ตัวอย่างการส่ง HTTP Request**

```http
METHOD: HttpMethod(value=POST)
Content-Type: application/json
Authorization: Bearer eyJhbGciOiJFZERTQSJ9.eyJzdWIiOiJhN2MwODczZS02Y2IxLTRjYmMtYWM2OS1hYTM3YWZlNzgwNTQiLCJpc3MiOiJodHRwczovL2lzc3Vlci1hcGktdGVzdC5ldGRhLm9yLnRoIiwiYXVkIjoiQUNDRVNTIn0.dH8bufDYHuMagSi3IpnUUcscYcZDtYgygFaDX5xY-LBSVM4eUpUj-a50rd9tFo4mMMHMo1OiPvFHguMV1KiSBw

BODY
Content-Type: application/json

{
    "credential_configuration_id": "TranscriptCredential",
    "proof": {
        "proof_type": "jwt",
        "jwt": "eyJhbGciOiJFZERTQSIsInR5cCI6IkpXVCIsImtpZCI6ImRpZDpqd2s6ZXlKcmRIa2lPaUpQUzFBaUxDSmpjbllpT2lKRlpESTFOVEU1SWl3aWEybGtJam9pZUhaVmNYZDRhbTQyYjBsdGNsVXROWGxETUZCdWFHOVZaa1pHZEhObFRGTTRNRmR6Wm5KcVJEWnNOQ0lzSW5naU9pSkNRMEpoV0RoM1drNURXamxGVm04eE5XMXJZVTVoVWs5V2RqRXhhRmcyVFZCM2VYVmZTVWR2T0VOWkluMCMwIn0.eyJhdWQiOiJodHRwczovL2lzc3Vlci1hcGktdGVzdC5ldGRhLm9yLnRoIiwiaWF0IjoxNzM2MDYwMTQ0LCJub25jZSI6ImZiMWQ1NDdlLWUwYTEtNDY4Ny05NTY2LWRkYTQxNzJlYmU1OCJ9.tlJM4POiJf-mzAW3w-rcJVGUwR8lWsYjMubD2ZRRGOqGPRupfetJdzmdUmersj1Tw88unGYKW-h2KcZEUBVADQ"
    }
}
```

**คำอธิบายค่าพารามิเตอร์**

| พารามิเตอร์ | Required | Type | รายละเอียด |
|-------------|----------|------|------------|
| credential_configuration_id | Required | String | ตัวระบุ Credential Configuration ตามที่ประกาศไว้ใน `credential_configurations_supported` ของ Metadata ผู้ออกเอกสาร ใช้แทนการระบุ `format` ตรง ๆ ในคำขอ |
| proof | Optional | Object | proof of possession ของคีย์เข้ารหัสที่เอกสารรับรองที่ออกจะถูกผูกมัดไว้ (ตรงกับ `cnf.jwk` ในตัว VC ตาม §5.1) จำเป็นหากมี proof_types_supported ใน credential_configurations_supported ของ metadata ของผู้ออกเอกสาร สำหรับเอกสารรับรองที่ขอ |
| proof_type | Required* | String | ค่าประเภทสตริงที่ระบุประเภทของ proof สำหรับคีย์ ค่าของพารามิเตอร์นี้กำหนดพารามิเตอร์อื่น ๆ ในวัตถุ proof ของคีย์ และกฎการประมวลผลที่เกี่ยวข้อง |
| credential_response_encryption | Optional | Object | ข้อมูลสำหรับเข้ารหัส Credential Response หากไม่มีพารามิเตอร์นี้ Credential Response ที่ส่งกลับมาจะไม่ถูกเข้ารหัส |

Credential Response อาจเป็นแบบทันทีหรือแบบรอการดำเนินการ:

- กรณีตอบกลับทันที: ผู้ออกเอกสารออกเอกสารรับรองที่ร้องขอได้ทันทีและส่งไปยังกระเป๋าเอกสาร
- กรณีรอตอบกลับ (Deferred Response): ผู้ออกเอกสารอาจไม่สามารถออกเอกสารรับรองที่ร้องขอได้ทันที และจะส่งพารามิเตอร์ transaction_id ให้กับกระเป๋าเอกสารเพื่อใช้ขอรับเอกสารรับรองเมื่อพร้อม รหัสสถานะ HTTP ต้องเป็น 202

การเข้ารหัสข้อมูล Credential Response:

- กรณีไม่มีการเข้ารหัส สื่อประเภทของคำตอบ (media type) ต้องถูกตั้งค่าเป็น application/json
- หากผู้ถือเอกสารร้องขอการเข้ารหัส Credential Response โดยส่ง credential_response_encryption ในคำขอ ผู้ออกเอกสารต้องเข้ารหัสข้อมูลใน Credential Response เป็น JWT โดยใช้พารามิเตอร์จาก credential_response_encryption และสื่อประเภทของคำตอบ (media type) ต้องถูกตั้งค่าเป็น application/jwt
- หากมีการร้องขอให้เข้ารหัสใน Credential Request แต่ Credential Response ไม่ได้ถูกเข้ารหัส กระเป๋าเอกสารควรปฏิเสธ Credential Response

**ตัวอย่างการส่ง HTTP Response ของผู้ออกเอกสาร (ไม่ได้เข้ารหัส)**

```http
Content-Type: application/json

{
    "credential": "eyJ0eXAiOiJkYytzZC1qd3QiLCJhbGciOiJFZERTQSJ9...LUpixVCWJk0eOt4CXQe1NXK~WyIzanFjYjY3ejl3a3NybmR2Iiwgc3R1ZGVudCIsICJOYXR0YXBvbmcgTWV0aGF3b24iXQ",
    "c_nonce": "fGFF7UkhLa",
    "c_nonce_expires_in": 86400
}
```

**คำอธิบายค่าพารามิเตอร์**

| พารามิเตอร์ | Required | Type | รายละเอียด |
|-------------|----------|------|------------|
| credential | Optional | String | ข้อมูลของเอกสารรับรองที่ออกให้ในรูปแบบ SD-JWT VC (`dc+sd-jwt`) ตามที่อธิบายใน §5.1 |
| transaction_id | Optional | String | เป็นค่าประเภทสตริงที่ระบุการออกเอกสารรับรองแบบ Deferred หากผู้ออกเอกสารไม่สามารถออกเอกสารรับรองได้ทันที โดยใช้ค่านี้ขอเอกสารรับรองในภายหลัง ค่านี้จะต้องถูกยกเลิกหลังจากที่กระเป๋าเอกสารได้รับเอกสารรับรองที่เกี่ยวข้องเรียบร้อยแล้ว |
| c_nonce | Optional | String | ค่าประเภทสตริงที่มี nonce ใช้ในการสร้าง proof of possession สำหรับ key proof |
| c_nonce_expires_in | Optional | Number | ระบุอายุการใช้งานของ c_nonce ในหน่วยวินาที |

## 5.4 Protocol สำหรับการใช้งาน VP

โพรโทคอล **OpenID for Verifiable Presentations (OID4VP) 1.0 Final** [11] ต่อยอดจากโพรโทคอล OID4VCI โดยมีการนำโพรโทคอล OpenID Connect (OIDC) มาใช้งานร่วมกันกับเอกสารสำแดงดิจิทัล (VP) ดังรูปที่ 12 การร้องขอ claim ใช้ **Digital Credentials Query Language (DCQL)** [11] แทนกลไก Presentation Exchange รุ่นก่อนหน้า โดยระบุ `format: "dc+sd-jwt"` และรายการ claim ที่ต้องการโดยตรงในคำร้องขอ

1. **OpenID Connect (OIDC):** เป็นโปรโตคอลที่ใช้ในการยืนยันตัวตนบนอินเทอร์เน็ต โดยสามารถใช้ในการลงชื่อเข้าใช้และรับรองตัวตนของผู้ใช้ได้ โดยมีการใช้ Access Token และ ID Token เพื่อยืนยันตัวตนและรับข้อมูลพื้นฐานของผู้ใช้
2. **Verifiable Presentations (VP):** เป็นรูปแบบการนำเสนอข้อมูลที่เชื่อถือได้ ที่สามารถใช้ในการยืนยันข้อมูลหรือข้อมูลตัวตนของบุคคลหรือองค์กร โดยมีการลงนามด้วยลายมือชื่อดิจิทัลและเข้ารหัสเพื่อให้มั่นใจได้ว่าข้อมูลมีความน่าเชื่อถือและไม่ได้ถูกแก้ไขโดยไม่ได้รับอนุญาต

![การใช้งาน OID4VP](images/p34-0.png)

**รูปที่ 12 การใช้งาน OID4VP**

![ขั้นตอนการทำ OID4VP](images/p35-0.png)

**รูปที่ 13 ขั้นตอนการทำ OID4VP**

### 5.4.1 กระบวนการทำงาน OID4VP

การตรวจสอบข้อมูลเอกสารสำแดงดิจิทัล (VP) ใช้โพรโทคอล OID4VP 1.0 Final พร้อม DCQL มีขั้นตอนการทำงานตามรูปที่ 13 ดังนี้

1. ผู้ตรวจสอบเอกสารส่ง Authorization Request ให้ผู้ถือเอกสาร
2. ผู้ถือเอกสารส่ง VP ให้ผู้ตรวจสอบผ่าน Authorization Response

#### (1) ผู้ตรวจสอบเอกสารส่ง Authorization Request ให้ผู้ถือเอกสาร

![สร้าง Authorization QR Code](images/p36-0.png)

**รูปที่ 14 สร้าง Authorization QR Code**

- Verifier สร้าง QR
- ผู้ถือเอกสารดำเนินการ Scan QR Code หรือ Upload QR Code ดังรูปที่ 14

**ตัวอย่างการ Decode QR ของ Verifier**

```json
{
    "client_id": "http://verifier.com/VP/callback",
    "client_id_scheme": "redirect_uri",
    "response_uri": "http://verifier.com/VP/callback",
    "response_type": "vp_token",
    "dcql_query": {
        "credentials": [
            {
                "id": "transcript",
                "format": "dc+sd-jwt",
                "meta": { "vct_values": ["TranscriptCredential"] },
                "claims": [
                    { "path": ["student"] },
                    { "path": ["gpa"] }
                ]
            }
        ]
    },
    "nonce": "fa2450d7-8ca5-40d2-8832-b03891c355be"
}
```

**คำอธิบายค่าพารามิเตอร์**

| พารามิเตอร์ | Required | Type | รายละเอียด |
|-------------|----------|------|------------|
| client_id | Required | String | ตัวระบุของผู้ตรวจสอบเอกสาร |
| client_id_scheme | Optional | String | รูปแบบของ client_id ที่ใช้ |
| response_uri | Optional | String | URI ที่ใช้ในการ callback |
| response_type | Required | String | ประเภทของ Grant Type ที่ต้องตอบกลับ ในการศึกษากำหนดค่าเป็น “vp_token” |
| dcql_query | Required* | Object | วัตถุ DCQL ที่ระบุ `credentials[]` แต่ละรายการประกอบด้วย `id`, `format: "dc+sd-jwt"`, `meta.vct_values` (ประเภทเอกสารที่ต้องการ) และ `claims[].path` (claim ที่ต้องการให้เปิดเผย) ต้องมีหากไม่มีค่า scope |
| client_metadata | Optional | JSON | JSON ที่มีค่า Metadata ของผู้ตรวจสอบเอกสาร ซึ่งต้องเข้ารหัสเป็น UTF-8 และต้องไม่มีหากพารามิเตอร์ client_metadata_uri มีอยู่ |
| client_metadata_uri | Optional | String | สตริงที่เป็น URL แบบ HTTPS ชี้ไปยัง JSON ที่มี Metadata ของผู้ตรวจสอบเอกสารได้ ต้องไม่มีหากพารามิเตอร์ client_metadata มีอยู่ |
| nonce | Required | String | ใช้ผูก Verifiable Presentation(s) กับธุรกรรมนั้น ๆ เพื่อความปลอดภัย |
| state | Optional | String | ใช้เพื่อเชื่อมโยง Request กับ Response |

#### (2) ผู้ถือเอกสารส่ง VP ให้ผู้ตรวจสอบผ่าน Authorization Response

![OID4VP Initiation](images/p38-0.png)

**รูปที่ 15 OID4VP Initiation**

- Verifier สร้าง QR Code และส่งให้ Holder ทำการ Scan QR หรือ Upload file QR Code ดังรูปที่ 15
- Holder จับคู่ VC ในเครื่องกับ `dcql_query` แล้วเลือก Disclosure ของ claim ที่ต้องการเปิดเผย ประกอบเป็น SD-JWT Presentation ตามโครงสร้างในตัวอย่างที่ 2
- Holder ส่งข้อมูล VP ให้ทาง Verifier

**ตัวอย่างการส่ง HTTP Response VP**

```http
Content-Type: application/x-www-form-urlencoded

vp_token={"transcript":"eyJ0eXAiOiJkYytzZC1qd3QiLCJhbGciOiJFZERTQSJ9...LUp~WyIzanFjYjY3ejl3a3NybmR2Iiwgc3R1ZGVudCIsICJOYXR0YXBvbmcgTWV0aGF3b24iXQ~eyJ0eXAiOiJrYitqd3QiLCJhbGciOiJFUzI1NiJ9...ziRjjF8Z3V5QkHEhZTFplqnqfk2BxEtZDA"}
state=sIiRkOc7THO
```

**คำอธิบายค่าพารามิเตอร์**

| พารามิเตอร์ | Required | Type | รายละเอียด |
|-------------|----------|------|------------|
| vp_token | Required | JSON Object | วัตถุ JSON ที่จับคู่ `id` ใน `dcql_query.credentials[]` เข้ากับ SD-JWT Presentation ของ credential นั้น (`Issuer-signed JWT~Disclosures~KB-JWT`) |
| state | Optional | String | ใช้เชื่อมโยง Response กับ Request เดิม |

### 5.4.2 กระบวนการ Resolve DID เพื่อตรวจสอบ VP

กระบวนการ Resolve DID เพื่อตรวจสอบ VP มีขั้นตอนดังรูปที่ 16

![Resolve DID](images/p39-0.png)

**รูปที่ 16 Resolve DID**

- ผู้ตรวจสอบเอกสารรับข้อมูล VP จากผู้ถือเอกสาร ดำเนินการ Decode VP และตรวจสอบความถูกต้องของโครงสร้าง
- ผู้ตรวจสอบเอกสารทำการส่งข้อมูล DID ID ไปตรวจสอบข้อมูลใน Registry
  - ข้อมูล DID => “did:tbsi:zv8fVB1i6S7MLMZX8wd3Ayo”
- การ Resolve ข้อมูลใน Registry มี 2 รูปแบบ
  - ทำการ Request ผ่าน DIF Universal Resolver
  - ทำการ Request ไปยัง Registry โดยตรง ซึ่งในรายงานฉบับนี้จะใช้วิธี “http get”
- ข้อมูลที่ได้จากการทำ Resolve คือข้อมูล DID Doc ของผู้ออกเอกสาร และส่งข้อมูล DID Doc กลับมาให้ผู้ตรวจสอบเอกสาร

การตรวจสอบข้อมูลใน Registry ในรายงานฉบับนี้จะระบุพารามิเตอร์ดังนี้

**ตัวอย่างการส่ง HTTP Request**

```http
GET 'http://example.verifier_url/GetDIDDoc?' Id=did:tbsi:zv8fVB1i6S7MLMZX8wd3Ayo
```

**คำอธิบายค่าพารามิเตอร์**

| พารามิเตอร์ | Required | รายละเอียด |
|-------------|----------|------------|
| Id | Required | หมายเลข DID |

ข้อมูลที่ได้จากการ Resolve และส่งกลับไปให้ Verifier

**ตัวอย่างการส่ง HTTP Response ของ Registry ไปยัง Verifier**

```json
{
    "status": 200,
    "message": "Success",
    "data": {
        "@context": "https://www.context.org",
        "id": "did:tbsi:zv8fVB1i6S7MLMZX8wd3Ayo",
        "verificationMethod": [
            {
                "id": "did:tbsi:zv8fVB1i6S7MLMZX8wd3Ayo#1XmYo2EnZwAL0giRxBWMADGk2Zei+TD2VKx7aWgWWAo=",
                "type": "JsonWebKey2020",
                "publicKeyJwk": {
                    "kty": "OKP",
                    "crv": "Ed25519",
                    "alg": "EdDSA"
                }
            }
        ],
        "assertionMethod": []
    }
}
```

**คำอธิบายค่าพารามิเตอร์**

| พารามิเตอร์ | Required | Type | รายละเอียด |
|-------------|----------|------|------------|
| Status | Required | Number | สถานะการเรียกใช้งาน |
| Message | Required | String | คำอธิบาย status |
| Data | Required | JSON | ข้อมูล DID DOC |

## 5.5 Universal Resolver

เพื่อให้ทุกระบบนิเวศ (ecosystem) ทำงานร่วมกันได้ จำเป็นต้องมีตัวกลางสำหรับรวบรวมข้อมูลที่ใช้ตรวจสอบและยืนยันความถูกต้องของข้อมูลสำคัญในรูปแบบมาตรฐานเดียวกัน จึงเกิดแนวคิดการใช้ DIF Universal Resolver

DIF Universal Resolver เป็นจุดเชื่อมต่อไปยังระบบนิเวศ (ecosystem) แต่ละแห่ง เพื่อให้การทำงานร่วมกัน (interoperability) เกิดขึ้นได้ แต่ละระบบนิเวศต้องลงทะเบียนใช้งาน DIF Universal Resolver โดยข้อมูลอยู่ในรูปแบบไฟล์อิเล็กทรอนิกส์มาตรฐานเดียวกันทั้งระบบนิเวศ และเมื่อต้องการตรวจสอบความถูกต้องของข้อมูลจากระบบนิเวศใด ก็สามารถสอบถามผ่าน DIF Universal Resolver ได้ทันที ทำให้แต่ละขั้นตอนดำเนินการได้รวดเร็วและเป็นมาตรฐานเดียวกัน

## 5.6 บทบาทและกระบวนการที่เกี่ยวข้องภายใต้ VC Ecosystem

รายงานฉบับนี้สรุปบทบาทและกระบวนการที่เกี่ยวข้องภายใต้ VC Ecosystem ดังรูปที่ 17 โดยมีรายละเอียดของแต่ละบทบาทดังนี้

![บทบาทของผู้ที่เกี่ยวข้องกับการใช้งานเอกสารรับรองดิจิทัล](images/p10-0.png)

**รูปที่ 17 บทบาทของผู้ที่เกี่ยวข้องกับการใช้งานเอกสารรับรองดิจิทัล**

**ก. ผู้ออกเอกสาร (Issuer) และกระบวนการที่เกี่ยวข้อง**

1. สร้าง DID Document ในรูปแบบ Legal Person
2. ลงทะเบียนใน Registry
3. เป็นผู้สร้าง VC
4. ยึดมาตรฐานการส่ง VC ในรูปแบบ Protocol OID4VCI 1.0 Final
5. สร้างและแสดง QR Code เพื่อให้ Holder เข้ามาร้องขอข้อมูลที่ต้องการ
6. ออก Token เพื่อใช้ในการยืนยันการส่ง VC
7. ตรวจสอบความถูกต้องของข้อมูลร้องขอ VC ที่ได้รับจาก Holder ส่งข้อมูล VC ในรูปแบบ SD-JWT VC (`dc+sd-jwt`) ให้กับทาง Holder

**ข. เจ้าของกระเป๋าเอกสารดิจิทัล (Wallet Holder) และกระบวนการที่เกี่ยวข้อง**

1. สร้าง DID Document ในรูปแบบ Natural Person
2. ร้องขอข้อมูลที่ต้องการจาก Issuer
3. Scan QR Code ที่ Issuer แสดงขึ้นมา เพื่อดำเนินการขอข้อมูล VC ในรูปแบบ Protocol OID4VCI 1.0 Final
4. ร้องขอ Token จาก Issuer เพื่อใช้ตรวจสอบการขอ VC
5. จัดเก็บ VC ที่ได้จาก Issuer ใน Wallet สำเร็จ
6. เลือก Verifier ที่ต้องการจะส่งต่อข้อมูล VC
7. สร้างข้อมูล VC ให้อยู่ในรูปแบบ VP โดยเลือก Disclosure ของ claim ที่ต้องการเปิดเผยและสร้าง Key Binding JWT
8. Scan QR Code ที่ Verifier แสดงขึ้นมา เพื่อดำเนินการส่งข้อมูล VP ในรูปแบบ Protocol OID4VP 1.0 Final พร้อม DCQL
9. ร้องขอ Token จาก Verifier เพื่อใช้ตรวจสอบการส่ง VP
10. ดำเนินการส่งข้อมูล VP ให้กับ Verifier สำเร็จ

**ค. ผู้ตรวจสอบเอกสาร (Verifier) และกระบวนการที่เกี่ยวข้อง**

1. ยึดมาตรฐานการรับ VP ในรูปแบบ Protocol OID4VP 1.0 Final พร้อม DCQL
2. สร้างและแสดง QR Code เพื่อให้ Holder เข้ามาร้องขอการส่งข้อมูล
3. ออก Token เพื่อใช้ในการยืนยันการรับ-ส่ง VP
4. รับข้อมูล VP จาก Holder สำเร็จ
5. ดึงข้อมูล (Resolve) จาก Registry จะดำเนินการผ่านขั้นตอน Decentralized Identity Foundation Universal Resolver (DIF Universal Resolver) เพื่อนำมาตรวจสอบความถูกต้องของผู้ออกข้อมูล VC
6. ดำเนินการตรวจสอบความถูกต้องของข้อมูล VP ที่ได้รับมา โดยตรวจสอบรายละเอียด ดังนี้
    - ถอดโครงสร้างเอกสารสำแดงดิจิทัล (Decode the VP) เป็นการแยกส่วนตามสัญลักษณ์ "~" ต้องประกอบด้วย
        - Issuer-signed JWT (คำอธิบายข้อมูลของ VC และข้อความยืนยันที่ถูกแฮชไว้ใน `_sd`)
        - Disclosures ของ claim ที่เปิดเผย
        - Key Binding JWT (ข้อพิสูจน์การครอบครองของผู้ถือเอกสาร)
    - ตรวจสอบลายมือชื่อดิจิทัล (Verify the Signature) จะตรวจสอบความถูกต้องของข้อมูลกุญแจสาธารณะของผู้ออกเอกสารรับรอง และตรวจลายมือชื่อ Key Binding JWT ด้วยกุญแจของผู้ถือเอกสาร
    - ตรวจสอบความถูกต้องของผู้ออกเอกสาร (Check Issuer's Credentials)
    - ตรวจสอบความถูกต้องของโครงสร้างเอกสารรับรองดิจิทัล (Validate the Credential Schema)
    - ตรวจสอบวันหมดอายุของเอกสารรับรองดิจิทัล (Validate the Credential Expiry)

---

**การนำทาง:** [⬅️ บทที่ 4 — แนวคิดและองค์ประกอบที่ส่งเสริมการทำงานร่วมกัน](04-interoperability-concepts.md) · [⬆️ สารบัญ ARF](README.md) · [บทที่ 6 — Decentralized Identifiers Methodologies ➡️](06-did-methodologies.md)
