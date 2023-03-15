# Mini_project
เอาข้อมูลมาจาก https://www.kaggle.com/datasets/nflovejoy/paycheck-protection-program-loan-data?select=public_150k_plus_230101.csv&sort=votes

# Author

1. Ratchaphon kuntipsombut 6510412013 DADS
2. 
## Data information 
File name : 1. public_150k_plus_230101.csv
Row = 968531 , Column = 53

Column ที่จำเป็นต้องรู้

1. LMI indicator คือ ซึ่งเป็นตัวบอกว่าผู้ขอกู้เงินมีรายได้ต่ำถึงปานกลางหรือไม่ โดย LMI Indicator เป็น N หมายถึงว่าผู้ขอกู้เงินไม่มีรายได้ต่ำถึงปานกลาง หรือไม่ได้รับการแสดงผลว่ามีรายได้ต่ำถึงปานกลางจากหน่วยงานที่เกี่ยวข้อง ซึ่งอาจเป็นบุคคลหรือกิจการที่มีรายได้สูงกว่ามาตรฐาน LMI ที่กำหนดไว้ตามกฎหมาย หรือไม่ตรงกับเกณฑ์ที่กำหนดในแต่ละประเทศหรือพื้นที่ดังกล่าว

2. Hubzone = เขตที่ไม่ค่อยได้ใช้ในอดีตที่มีความยากในการประกอบธุรกิจ

3. JobsReported = จำนวนพนักงานในบริษัท
4. BusinessAgeDescription = อายุของบริษัท (Existing ,Newbusiness , Start-up)

# Paycheck Protection Program Loan Data(PPP) คืออะไร ?                                                                                        
เป็นโปรแกรมเงินกู้ที่จัดตั้งขึ้นโดย US Small Business Administration (SBA) เพื่อตอบสนองต่อการระบาดใหญ่ของ Covid-19 
โดยจุดมุ่งหมายของ PPP คือการให้ความช่วยเหลือทางการเงินแก่ธุรกิจขนาดเล็กในรูปแบบของเงินที่ forgivable loans 
Forgivable loans คือ เงินที่ไม่ต้องคืนกลับในเงื่อนที่ PPP กำหนดไว้ เช่น ค่าจ้างเงินเดือน ค่าเช่าที่ ค่าสาธารณูปโภค และดอกเบี้ยจำนอง ในช่วงระยะเวลาที่กำหนด และมีข้อกำหนดตามที่ SBA ที่ได้กำหนดแนวทางในแต่ขั้นไว้

# OBJECTIVE: Use information from the dataset to answer the following question
1. ค่าเฉลี่ยของเงินที่ยืมไปในแต่ละพื้นที่ว่าในแต่ละรัฐ รัฐไหนยืมมากที่สุดเรียงตามลำดับ
2. ค่าเฉลี่ยของพนักงานในแต่ละรัฐ
3. ค่าเฉลี่ยของคนที่
4. หาว่าในปี 2020 และ 2021 การกู้ยืมมีแนวโน้วเป็นยังไง โดยดูจากจำนวนของคนกู้ในแต่ละปี

# DATA COLLECTION
## 1. Import Data
```df = pd.read_csv(r"C:\Users\User\Desktop\mini-project\public_150k_plus_230101.csv")```




