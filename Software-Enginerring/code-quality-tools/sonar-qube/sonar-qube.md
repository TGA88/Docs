## SonarQube Quality Gate Metrics

### New Code Metrics

| Metrics | Grade A (ดีเยี่ยม) | Grade B (ดี) | Grade C (พอใช้) | Grade D (ต้องปรับปรุง) | Grade E (แย่) |
|---------|-------------------|--------------|----------------|---------------------|--------------|
| Technical Debt | ≤ 120 นาที (2 ชม.) | ≤ 240 นาที (4 ชม.) | ≤ 480 นาที (1 วัน) | ≤ 720 นาที (1.5 วัน) | > 720 นาที |
| Coverage on New Code | ≥ 85% | ≥ 75% | ≥ 65% | ≥ 55% | < 55% |
| Duplication on New Code | ≤ 3% | ≤ 6% | ≤ 8% | ≤ 12% | > 12% |
| New Blocker Issues | 0 | 0 | 0 | 0 | ≥ 1 |
| New Critical Issues | 0 | ≤ 1 | ≤ 2 | ≤ 3 | > 3 |
| New Major Issues | ≤ 2 | ≤ 5 | ≤ 10 | ≤ 15 | > 15 |

### Overall Code Metrics

| Metrics | Grade A (ดีเยี่ยม) | Grade B (ดี) | Grade C (พอใช้) | Grade D (ต้องปรับปรุง) | Grade E (แย่) |
|---------|-------------------|--------------|----------------|---------------------|--------------|
| Overall Technical Debt | ≤ 480 นาที (1 วัน) | ≤ 960 นาที (2 วัน) | ≤ 1440 นาที (3 วัน) | ≤ 2400 นาที (5 วัน) | > 2400 นาที |
| Overall Coverage | ≥ 80% | ≥ 70% | ≥ 60% | ≥ 50% | < 50% |
| Overall Duplication | ≤ 5% | ≤ 8% | ≤ 10% | ≤ 15% | > 15% |
| Blocker Issues | 0 | 0 | 0 | ≤ 1 | > 1 |
| Critical Issues | 0 | ≤ 2 | ≤ 5 | ≤ 10 | > 10 |
| Major Issues | ≤ 5 | ≤ 10 | ≤ 20 | ≤ 30 | > 30 |

### คำอธิบายเพิ่มเติม

| หัวข้อ | คำอธิบาย |
|-------|----------|
| Technical Debt | เวลาที่ประมาณว่าต้องใช้ในการแก้ไขปัญหาทางเทคนิคทั้งหมด |
| Coverage | เปอร์เซ็นต์ของโค้ดที่ถูกทดสอบด้วย Unit Tests |
| Duplication | เปอร์เซ็นต์ของโค้ดที่ซ้ำซ้อน |
| Blocker Issues | ปัญหาร้ายแรงที่ต้องแก้ไขทันที เช่น Security Vulnerabilities |
| Critical Issues | ปัญหาสำคัญที่ควรแก้ไขโดยเร็ว |
| Major Issues | ปัญหาที่ควรได้รับการแก้ไขเพื่อปรับปรุงคุณภาพโค้ด |

### แนวทางการใช้งาน

1. **New Code**: ใช้เกณฑ์ที่เข้มงวดกว่าเพื่อป้องกันการเพิ่มปัญหาใหม่
2. **Overall Code**: ใช้เกณฑ์ที่ยืดหยุ่นกว่าสำหรับโค้ดเดิม
3. **การเริ่มต้น**: แนะนำให้เริ่มที่ Grade C แล้วค่อยๆ ปรับไปสู่ Grade A
4. **การติดตาม**: ตรวจสอบ metrics ทุกครั้งที่มีการ deploy
5. **การปรับปรุง**: ทบทวนและปรับเกณฑ์ทุก 3-6 เดือน