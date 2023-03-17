# Mini Project

Paycheck Protection Program Loan Data(PPP)

# Author

1. Ratchaphon kuntipsombut 6510412013 DADS
2. Worapat Phinyawan 6510412014 DADS
## Data information 

เอาข้อมูลมาจาก https://www.kaggle.com/datasets/nflovejoy/paycheck-protection-program-loan-data?select=public_150k_plus_230101.csv&sort=votes

File name : 1. public_150k_plus_230101.csv

Row = 968531 , Column = 53

Column ที่จำเป็นต้องรู้

1. LMI indicator คือ ซึ่งเป็นตัวบอกว่าผู้ขอกู้เงินมีรายได้ต่ำถึงปานกลางหรือไม่ โดย LMI Indicator เป็น N หมายถึงว่าผู้ขอกู้เงินไม่มีรายได้ต่ำถึงปานกลาง หรือไม่ได้รับการแสดงผลว่ามีรายได้ต่ำถึงปานกลางจากหน่วยงานที่เกี่ยวข้อง ซึ่งอาจเป็นบุคคลหรือกิจการที่มีรายได้สูงกว่ามาตรฐาน LMI ที่กำหนดไว้ตามกฎหมาย หรือไม่ตรงกับเกณฑ์ที่กำหนดในแต่ละประเทศหรือพื้นที่ดังกล่าว
2. Hubzone = เขตที่ไม่ค่อยได้ใช้ในอดีตที่มีความยากในการประกอบธุรกิจ
3. JobsReported = จำนวนพนักงานในบริษัท
4. BusinessAgeDescription = อายุของบริษัท (Existing ,Newbusiness , Start-up)
5. ProcessingMethod = แบ่งออกเป็น PPP(การกู้ยืมครั้งแรก) และ PPS(การกู้ยืมครั้งที่ 2) 

# Paycheck Protection Program Loan Data(PPP) คืออะไร ?                                                                                        
Paycheck Protection Program (PPP) คือโครงการของรัฐบาลสหรัฐอเมริกา (US Government) ที่เป็นส่วนหนึ่งของแผนช่วยเหลือเศรษฐกิจจากผลกระทบของโควิด-19 โดยโครงการนี้มีวัตถุประสงค์เพื่อช่วยเหลือธุรกิจขนาดเล็ก โดยให้สินเชื่อพิเศษในการจัดหาทุนทำธุรกิจเพื่อช่วยเหลือในการจ่ายเงินเดือนของพนักงานและค่าใช้จ่ายอื่นๆ เช่น ค่าเช่าอาคารสำนักงาน ค่าน้ำ ค่าไฟฟ้า และค่าใช้จ่ายอื่นๆ โดยสินเชื่อนี้สามารถขอได้จากธนาคารหรือสถาบันการเงินอื่นๆ ที่เป็นส่วนหนึ่งของโครงการนี้ โดยส่วนใหญ่จะมีอัตราดอกเบี้ยที่ต่ำกว่าปกติและส่วนบางส่วนอาจถูกยกเว้นไม่ต้องชำระเงินคืน โดยหลังจากที่ได้รับการอนุมัติสินเชื่อแล้ว ธุรกิจต้องใช้เงินในวัตถุประสงค์ที่กำหนดไว้ในโครงการภายในเวลา 8-24 สัปดาห์ โดยต้องส่งเอกสารแสดงการใช้เงินกลับมายืนยันต่อธนาคารเพื่อไม่ให้เกิดค่าใช้จ่ายเพิ่มเติมในภายหลัง

# OBJECTIVE: Use information from the dataset to answer the following question
1. ค่าเฉลี่ยของเงินที่ยืมไปในแต่ละพื้นที่ว่าในแต่ละรัฐเป็นยังไง รัฐไหนยืมมากที่สุดเรียงตามลำดับ
2. ค่าเฉลี่ยของพนักงานในแต่ละรัฐเป็นเท่าไร
3. ธุรกิจที่อยู่ในพื้นที่ Hubzone ได้เงินกู้โดยเฉลี่ยมากกว่าธุรกิจไม่ได้อยู่ในพื้นที่ Hubzone หรือไม่
4. ลักษณะของธุรกิจที่มีการกู้ยืมมากที่สุดมีลักษณะเป็นอย่างไร
5. หาว่าในปี 2020 และ 2021 การกู้ยืมมีแนวโน้วเป็นยังไง โดยดูจากจำนวนของคนกู้ในแต่ละปี
6. ธนาคารไหนปล่อยเงินกู้โดยเฉลี่ยให้กับธุรกิจมากที่สุด

# DATA COLLECTION
## 1. Import Data
```df = pd.read_csv(r"C:\Users\User\Desktop\mini-project\public_150k_plus_230101.csv")```
## 2. Cleansing data
ขั้นแรกเราทำการดูค่า NaN 

```
null_percentages = df.isna().sum() / len(df) * 100
null_columns = null_percentages[null_percentages > 80].index.tolist()
print(f"Column have NaN more than 80% = {null_columns}")
```
เราจะพบว่า Column ที่มี NaN มากกว่า 80% จะมี 'FranchiseName', 'MORTGAGE_INTEREST_PROCEED', 'RENT_PROCEED', 'REFINANCE_EIDL_PROCEED', 'HEALTH_CARE_PROCEED', 'DEBT_INTEREST_PROCEED', 'NonProfit' และที่มี NaN 60% UTILITIES_PROCESS 

ต่อมา Drop column โดยใช้
```
df = df.drop(['FranchiseName', 'MORTGAGE_INTEREST_PROCEED', 'RENT_PROCEED', 'REFINANCE_EIDL_PROCEED', 'HEALTH_CARE_PROCEED', 'DEBT_INTEREST_PROCEED', 'NonProfit','UTILITIES_PROCEED'],axis=1)
```
แล้วเราก็ดูว่าเหลือ column ไหนที่ NaN เหลืออยู่
```
df.isnull().sum()
```
จากที่เห็นมี NaN ไม่มากก็เลยใช่และคิดว่ามันจะไม่มีกับผลลัพธ์ที่ออกมา
```
df = df.dropna()
```
โดยจะเหลือ row = 916738 , column = 45

ดูผลสรุปของ Statistic ของ Column ที่ต้องใช้
```
pd.options.display.float_format = "{:,.2f}".format
df.describe()[['InitialApprovalAmount','CurrentApprovalAmount']]
```
จากผลที่สรุปที่ได้มาจะเห็นว่าค่า std มีค่ามากกว่าค่า mean และ max มีมากสุด 10 ล้าน เดาได้เลยว่าเป็นอาจข้อมูลที่กรอกผิด เราเลยใช้ function ในการ Fitler Outlier  
```
#remove outliner
def filter_outliner(df, column_name):
    q_low = df[column_name].quantile(0.01)
    q_hi  = df[column_name].quantile(0.99)
    df_filtered = df[(df[column_name] < q_hi) & (df[column_name] > q_low)]
    return df_filtered
```
ใช้ boxplot เพื่อดู การกระจายตัวของข้อมูล (distribution) และสรุปค่าสถิติ ก่อนใช้และหลังใช้ funtion

ก่อนใช้ function

![image](https://user-images.githubusercontent.com/110782963/225678993-997d8d2e-67ee-42a8-bd99-920a7204cfa0.png)

![image](https://user-images.githubusercontent.com/110782963/225341082-7c41195f-14f1-43b0-a63c-11bbd4e87b94.png)

หลังใช้ function

![image](https://user-images.githubusercontent.com/110782963/225679155-d8aa346b-de76-4cbe-bce6-329973baab70.png)

![image](https://user-images.githubusercontent.com/110782963/225341416-29fdd34a-28c6-403d-ad4a-3323ab25921c.png)

โดยที่เราจะเหลือข้อมูลอยู่ Row = 879666 , column = 48 จากทั้งหมด Row = 968531 , Column = 53

# 3. Exploratory data analysis (EDA)

### Top 10 state for PPP and PPS

```
plt.figure(figsize = (15,6))
ax=sns.countplot(x="BorrowerState", data=df ,order = df['BorrowerState'].value_counts().index[:10], )
plt.title("Top 10 State for PPP and PPS")
```

![image](https://user-images.githubusercontent.com/110782963/225342487-8af0584e-9d61-4b7d-acdd-f75b3f0998d8.png)

Note : สรุปได้ว่าที่เมือง CA = California มีจำนวนบริษัทที่กู้ยืมมากที่สุด รองลงมาเป็น Tx(Texas) และ NY(New York) ตามลำดับ

### Top 5 ธนาคารที่ให้กู้ยืมมากที่สุด
```
sns.countplot(data = df, y = 'OriginatingLender', order = df['OriginatingLender'].value_counts().index[:5], color = 'Green')
```

![image](https://user-images.githubusercontent.com/110782963/225366799-07672b75-030c-48ec-80fe-b13f38ef3151.png)

Note : ธนาคาร JPMorgan chase Bank มีการปล่อยให้กู้ยืมมากที่สุด

### จำนวนของ LMI ที่ เป็น Y และ N
```
df_count_LMI = df['LMIIndicator'].value_counts()
display(df_count_LMI)
print("\n--------------------------------------\n")

sns.countplot(data = df, 
              x = 'LMIIndicator', 
              order = df['LMIIndicator'].value_counts().index, 
              color = 'Yellow')

plt.xlabel('LMIIndicator(Yes , NO)')
plt.ylabel('Count LMI')
```

![image](https://user-images.githubusercontent.com/110782963/225368118-ef56caca-481b-4194-a22a-b657a395565a.png)

Note : ดูจากกราฟจำนวนที่เป็น N มากกว่าที่ เป็น Y จึงสรุปได้ว่า ธูรกิจที่มีรายได้ต้่ากว่าปานกลางมีค่าน้อยกว่าธุรกิจที่ไม่มีรายได้ต่ำกว่าปานกลาง โดยที่ LMI indicator ที่เป็น N มีมากถึง 73.77%

### จำนวนของธุรกิจที่อยู่ในพื้นที่ Hubzone และ No-Hubzone

```
df_count_Hubzone = df['HubzoneIndicator'].value_counts()
display(df_count_Hubzone)
print("\n--------------------------------------\n")
sns.countplot(data = df, 
              x = 'HubzoneIndicator', 
              order = df['HubzoneIndicator'].value_counts().index, 
           
              color = 'Green')
plt.title('HubzoneIndicator')
```
![image](https://user-images.githubusercontent.com/110782963/225369464-b612f0ec-1791-476a-8aae-e5749382c6a6.png)

Note : สรุปได้ว่าจากกราฟ  N > Y แสดงว่า เขตที่อยู่นอก Hubzone มากกว่าเขตที่อยู่ในพิ้นที่ Hubzone โดยที่เขตที่ไม่ใช่ Hubzone มีมากถึง 73.14%

### จำนวนอายุของธุรกิจ และ LMI
```
f_fittered = df.groupby(['BusinessAgeDescription', 'LMIIndicator'])['BusinessAgeDescription'].apply(lambda x : x.count()).sort_values(ascending=True).plot(kind = 'barh',
																						color = 'Green' , 
																						alpha = 0.6 )
```

![image](https://user-images.githubusercontent.com/110782963/225371468-d4715fb2-c048-41bf-b8b3-9c65752ffeda.png)

สรุปได้ว่า ส่วนใหญ่จำนวนของธุรกิจที่มีอยู่แล้วหรือมากกว่า 2 ปี และ มี IMI indicator มีผลเป็น N ที่บอกว่าไม่ใช่เป็นบริษัทรายได้ต่ำกว่าปานกลาง 

### จำนวน BusinessType 

```
sns.countplot(data = df, y = 'BusinessType', order = df['BusinessType'].value_counts().index)
```

![image](https://user-images.githubusercontent.com/110782963/225372209-02037779-5e1b-4a06-b7d1-c9c22fc1c80c.png)


สรุปได้ว่า ส่วนใหญ่แล้วเป็นธุรกิจประเภท Corporation , LLC , Subchapter S Corporation













## Q1 ค่าเฉลี่ยของเงินที่ยืมไปในแต่ละพื้นที่ว่าในแต่ละรัฐ รัฐไหนยืมมากที่สุดเรียงตามลำดับ
```
df_statistic  = df.groupby("BorrowerState")['CurrentApprovalAmount'].agg(['mean','median','sum','count']).sort_values(by= ['mean','sum','median','count'],ascending=False)

display(df_statistic.head())

print('\n-------------------------------------------------------\n')

display(df_statistic.tail())

```

![image](https://user-images.githubusercontent.com/110782963/225691855-c8210da2-9eed-4a1a-836d-a352763deb7f.png)

Note : สรุปได้ว่า DC (Washington, D.C.) มีค่าเฉลี่ยมากที่สุด และ AS(American samoa) ที่มีค่าเฉลี่ยน้อยที่สุด



## Q2 ค่าเฉลี่ยของพนักงานในแต่ละรัฐเป็นเท่าไร
```
df.groupby("BorrowerState")['JobsReported'].mean().sort_values().plot(
    kind="bar",
    title = "Mean JobsReported  In Each State",
    alpha = 0.3,
    legend=True,
)

df.groupby("BorrowerState")["JobsReported"].median().sort_values().plot(
    kind="line",
    title = "Mean JobsReported In Each State",
    legend=True,
    color="red",
    rot=90,
    alpha=.8,
    figsize=(15,6) # Determines the size 
)
plt.ylabel("JobsReported") 
plt.legend(["Median", "Mean"]);

```

![image](https://user-images.githubusercontent.com/110782963/225367327-e4e5fd91-bace-4335-9420-70277742f426.png)

จากกราฟ สรุปได้ว่าในรัฐ AS( American Samoa) มีจำนวนพนักงานเฉลี่ยมากที่สุด ถึงแม้ว่าจะมีประชากรน้อยก็ตาม

## Q3 ธุรกิจที่อยู่ในพื้นที่ Hubzone ได้เงินกู้โดยเฉลี่ยมากกว่าธุรกิจไม่ได้อยู่ในพื้นที่ Hubzone หรือไม่

```
# หาค่าเฉลี่ย Approval_Amount ของ Hubzone เป็น Y และ N
avg_y = df[df['HubzoneIndicator'] == 'Y']['CurrentApprovalAmount'].mean()
avg_n = df[df['HubzoneIndicator'] == 'N']['CurrentApprovalAmount'].mean()

# แสดงผลค่าเฉลี่ย Approval_Amount ของ Hubzone เป็น Y และ N ที่มีทศนิยม 2 ตำแหน่ง
print("ค่าเฉลี่ย Approval_Amount ของ Hubzone เป็น Y: ", round(avg_y, 2))
print("ค่าเฉลี่ย Approval_Amount ของ Hubzone เป็น N: ", round(avg_n, 2))

```

![image](https://user-images.githubusercontent.com/110782963/225650558-7d540f80-2db8-4f14-885b-673037260132.png)

Note : พบว่าธุรกิจที่อยู่ในพื้นที่ Hubzone ได้เงินกู้โดยเฉลี่ยมากกว่าธุรกิจไม่ได้อยู่ในพื้นที่ Hubzone แต่ไม่มากนัก
```
filtered_df = df[(df['HubzoneIndicator'] == 'Y') ]
mean_approval_amount_hubzone = filtered_df['CurrentApprovalAmount'].mean()
count_y_h = filtered_df.shape[0]

print(f"ค่าเฉลี่ย Approval Amount ของคนที่ทำธุรกิจในพื้นที่ Hubzone {mean_approval_amount_hubzone:.2f} $")
print(f"จำนวนธุรกิจที่อยู่ในพื้นที่ Hubzone คือ {count_y_h}")

```

![image](https://user-images.githubusercontent.com/110782963/225651032-7d64bf6d-2e2a-4f28-ae07-f814cee2e134.png)

```
filtered_df = df[ (df['LMIIndicator'] == 'Y')]

mean_approval_amount_lmi = filtered_df['CurrentApprovalAmount'].mean()

count_y_l = filtered_df.shape[0]

print(f"ค่าเฉลี่ย Approval Amount ของคนที่ทำธุรกิจในพื้นที่ LMI คือ {mean_approval_amount_lmi:.2f} $")
print(f"จำนวนธุรกิจที่อยู่ในพื้นที่ LMI คือ {count_y_l}")

```

![image](https://user-images.githubusercontent.com/110782963/225651289-8fb3cfe8-a771-4611-992d-57fc80283009.png)

```
filtered_df = df[(df['HubzoneIndicator'] == 'Y') & (df['LMIIndicator'] == 'Y')]

mean_approval_amount = filtered_df['CurrentApprovalAmount'].mean()

count_y = filtered_df.shape[0]

print(f"ค่าเฉลี่ย Approval Amount ของคนที่ตอบ Y จาก Hubzone และ LMI คือ {mean_approval_amount:.2f} $")
print(f"จำนวนธุรกิจที่อยู่ในพื้นที่ Hubzone และ LMI คือ {count_y}")

```

![image](https://user-images.githubusercontent.com/110782963/225651818-31c18f8d-2227-431a-aace-eddd9b70eaca.png)

Note : จะเห็นว่า Approval Amount ของธุรกิจที่อยู่ในพื้นที่ Hubzone และธุรกิจที่อยู่ในกลุ่ม  LMI นั้นไม่ต่างกันมากนักแต่ด้วยธุรกิจที่อยู่ในกลุ่ม LMI และอยู่ในพื้นที่ Hubzone มีจำนวนไม่น้อยแต่ได้จำนวนเงินไม่ต่างกับสองกลุ่มก่อนหน้า

## Q4 ลักษณะของธุรกิจที่มีการกู้ยืมมากที่สุดมีลักษณะเป็นอย่างไร

```

df.groupby(['BusinessAgeDescription','LMIIndicator','HubzoneIndicator',"BusinessType"]).size().reset_index(name='count').sort_values('count', ascending=False).head()

```

![image](https://user-images.githubusercontent.com/110782963/225654152-b3ba00b1-d999-4eff-a9e5-49713abfeba6.png)

Note : พบว่าธุรกิจส่วนใหญ่ที่มากู้ยืมมีอายุมากกว่า 2 ปี โดยที่ 3 อันดับแรกจะเป็นบริษัทประเภท Corporation, Limited Liability Company(LLC) และ Subchapter S Corporation โดยจะมี LMI เป็น N และ Hubzone เป็น N หรือก็คือทั้ง 3 กลุ่มเป็นธุรกิจที่มีรายได้สูงกว่ามาตรฐาน LMI ไม่อยู่ในย่าน Hubzone ไม่ยากในการทำธุรกิจและไม่ใช่ย่านที่มีอัตราการว่างงานสูง

Action plan : อาจจะเป็นการจัดโปรโมชั่นลดอัตราดอกเบี้ยเงินกู้ให้กับธุรกิจที่มีอายุมากกว่า 2 ปี หรือจะเน้นไปที่กลุ่มธุรกิจ 3 ประเภทนั้นเพื่อเพิ่มโอกาสในการกู้ยืมเงินในอนาคต









## Q5 หาว่าในปี 2020 และ 2021 การกู้ยืมมีแนวโน้วเป็นยังไง โดยดูจากจำนวนของคนกู้ในแต่ละปี

ตอนแรกเราดูจากข้อมูล ที่ column Dateapprove ยังไม่เป็น type ตามที่เราต้องการ เราจึงใช้ 

```df['DateApproved'] = pd.to_datetime(df['DateApproved'])``` 

ในการเปลี่ยน data type ให้เป็น datetime

### เรามาดูรายปีว่าในแต่ปีมีคนกู้มากน้อยแค่ไหน

```
df['year'] = df['DateApproved'].dt.year
df_grouped = df.groupby(['year'])['CurrentApprovalAmount'].apply(lambda x : x.count()).sort_values()

df_grouped.plot(kind = 'bar', 
                color = 'blue' ,
                title= 'Number of businesses approved for PPP loan by year and LMI Indicator',
                xlabel= 'Year',
                ylabel= 'Count',
                )
```

![image](https://user-images.githubusercontent.com/110782963/225524422-edc84541-473e-4741-8eda-8c48fdf79258.png)

จากกราฟสรุปได้ว่า ในปี 2020 มีคนกู้ยืม มากกว่าปี 2021 

### เรามาลองแบ่งให้ละเอียดขึ้นโดยแบ่งเป็น ปี-เดือน โดยมี LMI เป็นตัวเปรียบเทียบในแต่ละปี

```
df['year_month'] = df['DateApproved'].dt.strftime('%Y-%m')

df_grouped = df.groupby(['year_month', 'LMIIndicator']).size().reset_index(name='count')

plt.figure(figsize=(15,6))
plt.bar(df_grouped[df_grouped['LMIIndicator'] == 'N']['year_month'], df_grouped[df_grouped['LMIIndicator'] == 'N']['count'], color='green', alpha=0.7 )
plt.bar(df_grouped[df_grouped['LMIIndicator'] == 'Y']['year_month'], df_grouped[df_grouped['LMIIndicator'] == 'Y']['count'], color='blue', alpha=0.7)
plt.xlabel('Year-Month')
plt.ylabel('Count')
plt.title('Number of businesses approved for PPP loan by year and month and LMI Indicator')
plt.legend(['Non-LMI', 'LMI'], loc='upper right')
plt.show()

```
![image](https://user-images.githubusercontent.com/110782963/225699526-eb3eb8cc-e48c-41b2-9baa-b6d6e54c821a.png)


Note : 
1. จากกราฟแสดงให้เห็นว่าการแบ่งกราฟตามปีและแสดงจำนวนธุรกิจที่เป็น LMI(N) และ LMI(Y) ในแต่ละเดือน จะเห็นได้ว่าจำนวนธุรกิจที่ เป็น LMI(N) มีมากกว่าจำนวนธุรกิจที่มี LMI(Y) ซึ่งสามารถอธิบายได้ว่าธุรกิจที่มีรายได้ต่ำกว่าปานกลางจะมีน้อยกว่าธุรกิจที่มีรายได้ปกติหรือสูง ในทุกเดือนๆ
2. จากกราฟจะเห็นได้ว่าในปี 2021 มีจำนวนการยืมน้อยลงเมื่อเทียบกับ ปี 2020 อาจจะเป็นสัญญาณในการฟื้นตัวของธุรกิจที่เป็นผลของโควิด-19 ทำให้ไม่ต้องการเงินสนับสนุนจาก โครงการ Paycheck Protection Program อีกต่อไป

Action plan : 
ศึกษาและวิเคราะห์เพิ่มเติมเกี่ยวกับสาเหตุและผลกระทบของการแบ่งแยกตาม LMI Indicator และพัฒนานโยบายเพื่อเปิดโอกาสให้กับธุรกิจที่มี LMI Indicator เป็น Y ที่มีฐานะทางการเงินที่มีฐานะการเงินต่ำกว่าปานกลางให้มีฐานะการเงินที่มากขึ้นหรือเท่ากับกิจการที่ LMI Indicator เป็น N ที่เป็นกลุ่มธุรกิจที่มีรายได้ปกติหรือสูง

## Q6 ธนาคารไหนปล่อยเงินกู้โดยเฉลี่ยให้กับธุรกิจมากที่สุด

```
result = df.groupby('ServicingLenderName')['CurrentApprovalAmount'].agg(['median','mean', 'count']).round(2)

df_mean = result.sort_values(['median'] , ascending=False)

display(df_mean)

```
![image](https://user-images.githubusercontent.com/110782963/225656578-2ba62c6a-e7bb-4414-bc85-81ead3af5f6c.png)

Note : จะเห็นว่าว่าธนาคารที่ปล่อยกู้เฉลี่ยมากที่สุดคือ Peapack-Gladstone Bank, Southeast Community Capital Corporation dba Pathway Lending, Western Cooperative CU ตามลำดับ 

Action plan : จากข้อมูลนี้เราอาจจะใช้เป็นทางเลือกให้กับธุรกิจที่ต้องการเงินกู้ที่มีจำนวนมากแต่อาจต้องศึกษาเกี่ยวกับกฏเกณฑ์และอัตราดอกเบี้ยของธนาคารนั้นๆเพิ่มเติม

```
df_count = result.sort_values(['count'] , ascending=False)

display(df_count)

```
![image](https://user-images.githubusercontent.com/110782963/225657261-5f709f84-3680-42b6-8ef2-ce3ff675ae22.png)


Note : ธนาคารที่ปล่อยกู้ให้ธุรกิจมากที่สุดคือ JPMorgan Chase Bank, National Association, Bank of America, , PNC Bank, Truist Bank ตามลำดับโดยที่ JPMorgan Chase Bank อนุมัติเงินกู้ให้กับธุรกิจถึง 45191 

Action plan : รายจากข้อมูลนี้เพื่อใช้แนะนำให้กับธุรกิจที่อาจจะไม่ชอบความยุ่งยากในการขออนุมัติหรือธุรกิจที่ไม่ได้ต้องการวงเงินกู้สูงมากนัก

# Challenge
1. ใช้เวลาในการทำความเข้าใจข้อมูล และเป็นเรื่องใหม่สำหรับพวกเรา ทำให้ใช้เวลานานในการทำเข้าใจ 
2. ในการ cleansing ข้อมูลไม่รู้ว่าเริ่มจากอะไรก่อน เลยจะเวลานานในส่วนนี้มาก และ ส่วนหนึ่งอาจจะมาจากการที่ไม่รู้การเขียน code ที่เหมาะสม ที่สามารถลดเวลาการเขียนลงได้
3. เราจะใช้ กราฟ อะไรดีในการอธิบายข้อมูล อาจจะเพราะว่าประสบการ์ณในการทำยังไม่มีมาก







