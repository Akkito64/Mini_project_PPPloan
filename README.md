# Mini_project
เอาข้อมูลมาจาก https://www.kaggle.com/datasets/nflovejoy/paycheck-protection-program-loan-data?select=public_150k_plus_230101.csv&sort=votes

# Author

1. Ratchaphon kuntipsombut 6510412013 DADS
2. Worapat Phinyawan 6510412014 DADS
## Data information 
File name : 1. public_150k_plus_230101.csv
Row = 968531 , Column = 53

Column ที่จำเป็นต้องรู้

1. LMI indicator คือ ซึ่งเป็นตัวบอกว่าผู้ขอกู้เงินมีรายได้ต่ำถึงปานกลางหรือไม่ โดย LMI Indicator เป็น N หมายถึงว่าผู้ขอกู้เงินไม่มีรายได้ต่ำถึงปานกลาง หรือไม่ได้รับการแสดงผลว่ามีรายได้ต่ำถึงปานกลางจากหน่วยงานที่เกี่ยวข้อง ซึ่งอาจเป็นบุคคลหรือกิจการที่มีรายได้สูงกว่ามาตรฐาน LMI ที่กำหนดไว้ตามกฎหมาย หรือไม่ตรงกับเกณฑ์ที่กำหนดในแต่ละประเทศหรือพื้นที่ดังกล่าว

2. Hubzone = เขตที่ไม่ค่อยได้ใช้ในอดีตที่มีความยากในการประกอบธุรกิจ

3. JobsReported = จำนวนพนักงานในบริษัท
4. BusinessAgeDescription = อายุของบริษัท (Existing ,Newbusiness , Start-up)

# Paycheck Protection Program Loan Data(PPP) คืออะไร ?                                                                                        
เป็นโปรแกรมเงินกู้ที่จัดตั้งขึ้นโดย US Small Business Administration (SBA) เพื่อตอบสนองต่อการระบาดใหญ่ของ Covid-19 w
โดยจุดมุ่งหมายของ PPP คือการให้ความช่วยเหลือทางการเงินแก่ธุรกิจขนาดเล็กในรูปแบบของเงินที่ forgivable loans 
Forgivable loans คือ เงินที่ไม่ต้องคืนกลับในเงื่อนที่ PPP กำหนดไว้ เช่น ค่าจ้างเงินเดือน ค่าเช่าที่ ค่าสาธารณูปโภค และดอกเบี้ยจำนอง ในช่วงระยะเวลาที่กำหนด และมีข้อกำหนดตามที่ SBA ที่ได้กำหนดแนวทางในแต่ขั้นไว้

# OBJECTIVE: Use information from the dataset to answer the following question
1. ค่าเฉลี่ยของเงินที่ยืมไปในแต่ละพื้นที่ว่าในแต่ละรัฐ รัฐไหนยืมมากที่สุดเรียงตามลำดับ
2. ค่าเฉลี่ยของพนักงานในแต่ละรัฐ
3. หาว่าในปี 2020 และ 2021 การกู้ยืมมีแนวโน้วเป็นยังไง โดยดูจากจำนวนของคนกู้ในแต่ละปี

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
จากผลที่สรุปที่ได้มาจะเห็นว่าค่า std มีค่ามากกว่าค่า mean และ max มีมากสุด 10 ล้าน เดาได้เลยว่าเป็น outliner แน่นอน เราเลยใช้ function ในการกำจัด Outlier ออกไป และสามารถใช้ boxplot เพื่อดู Outlier
```
#remove outliner
def filter_outliner(df, column_name):
    q_low = df[column_name].quantile(0.01)
    q_hi  = df[column_name].quantile(0.99)
    df_filtered = df[(df[column_name] < q_hi) & (df[column_name] > q_low)]
    return df_filtered
```
ใช้ boxplot เพื่อดู การกระจายตัวของข้อมูล (distribution)

ก่อนใช้ function

![image](https://user-images.githubusercontent.com/110782963/225341082-7c41195f-14f1-43b0-a63c-11bbd4e87b94.png)

หลังใช้ function

![image](https://user-images.githubusercontent.com/110782963/225341416-29fdd34a-28c6-403d-ad4a-3323ab25921c.png)

# 3. Exploratory data analysis (EDA)

### Top 10 state for PPP and PPS

```
plt.figure(figsize = (15,6))
ax=sns.countplot(x="BorrowerState", data=df ,order = df['BorrowerState'].value_counts().index[:10], )
plt.title("Top 10 State for PPP and PPS")
```

![image](https://user-images.githubusercontent.com/110782963/225342487-8af0584e-9d61-4b7d-acdd-f75b3f0998d8.png)

### Top 5 ธนาคารที่ให้กู้ยืมมากที่สุด
```
sns.countplot(data = df, y = 'OriginatingLender', order = df['OriginatingLender'].value_counts().index[:5], color = 'Green')
```

![image](https://user-images.githubusercontent.com/110782963/225366799-07672b75-030c-48ec-80fe-b13f38ef3151.png)

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

ดูจากกราฟจำนวนที่เป็น N มากกว่าที่ เป็น Y จึงสรุปได้ว่า ธูรกิจที่มีรายได้ต้่ากว่าปานกลางมีค่ามากกว่าธุรกิจที่มีรายได้สูง

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

ดูจากกราฟจำนวนที่เป็น N มากกว่าที่ เป็น Y จึงสรุปได้ว่า ธูรกิจที่อยู่ไม่อยู่ในพื้นที่ Hubzone มากกว่า ธุรกิจที่อยู่ในพื้นที่ Hubzone

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


สรุปได้ว่า ส่วนเป็นธุรกิจแล้ว Corporation , LLC , Subchapter S Corporation













## Q1 ค่าเฉลี่ยของเงินที่ยืมไปในแต่ละพื้นที่ว่าในแต่ละรัฐ รัฐไหนยืมมากที่สุดเรียงตามลำดับ
```
q_median = df.groupby("BorrowerState")['InitialApprovalAmount','CurrentApprovalAmount'].apply(lambda x : x.median()).sort_values(by = ["InitialApprovalAmount",'CurrentApprovalAmount'],ascending=True).plot(kind = 'barh' , figsize = (15,15)    )
plt.title('Mean Borrowerstate by Initail and Current')
plt.xlabel('State')
plt.ylabel('Amount')
plt.show()

```
![image](https://user-images.githubusercontent.com/110782963/225366400-f64c9d0c-0639-4628-8e64-f6ce1537fbe9.png)

## Q2 ค่าเฉลี่ยของพนักงานในแต่ละรัฐ
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

## Q3 หาว่าในปี 2020 และ 2021 การกู้ยืมมีแนวโน้วเป็นยังไง โดยดูจากจำนวนของคนกู้ในแต่ละปี

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

plt.figure(figsize=(10,6))
plt.bar(df_grouped[df_grouped['LMIIndicator'] == 'N']['year_month'], df_grouped[df_grouped['LMIIndicator'] == 'N']['count'], color='green', alpha=0.7 )
plt.bar(df_grouped[df_grouped['LMIIndicator'] == 'Y']['year_month'], df_grouped[df_grouped['LMIIndicator'] == 'Y']['count'], color='blue', alpha=0.7)
plt.xlabel('Year-Month')
plt.ylabel('Count')
plt.title('Number of businesses approved for PPP loan by year and month and LMI Indicator')
plt.legend(['Non-LMI', 'LMI'], loc='upper right')
plt.show()

```
![image](https://user-images.githubusercontent.com/110782963/225525120-ef34692c-0c2d-45cd-8dcf-0143738f5bf5.png)

Note :
1. จำนวนธุรกิจที่ No LMI จะมีมากกว่าจำนวนธุรกิจที่มี LMI Indicator โดยมีสัดส่วนประมาณ 60 : 40  
2. ความถี่ของแต่ละ category สามารถแสดงได้ด้วยความสูงของแท่งในกราฟ ซึ่งแท่งสีฟ้าแสดงจำนวนธุรกิจที่ LMI และแท่งเขียวแสดงจำนวนธุรกิจที่ No LMI
3. จากกราฟแสดงให้เห็นว่าการแบ่งกราฟตามปีและแสดงจำนวนธุรกิจที่มีและไม่มี LMI Indicator ในแต่ละปี จะเห็นได้ว่าจำนวนธุรกิจที่ไม่มี LMI Indicator มีมากกว่าจำนวนธุรกิจที่มี LMI Indicator โดยทั่วไป ซึ่งสามารถอธิบายได้ว่ากิจการที่ไม่มี LMI Indicator มีฐานะทางการเงินที่แข็งแกร่งกว่ากิจการที่มี LMI Indicator อยู่บ้าง 
4. จากกราฟจะเห็นได้ว่าในปี 2021 มีจำนวนการยืมน้อยลงเมื่อเทียบกับ ปี 2020 อาจจะเป็นสัญญาณในการฟื้นตัวของธุรกิจที่เป็นผลของโควิด-19 ทำให้ไม่ต้องการเงินสนับสนุนจาก โครงการ Paycheck Protection Program อีกต่อไป

Action plan ที่เราจะต่อการที่ได้ข้อมูลนี้มา
1. ศึกษาและวิเคราะห์เพิ่มเติมเกี่ยวกับสาเหตุและผลกระทบของการแบ่งประเภทตาม LMI Indicator และพัฒนานโยบายเพื่อเปิดโอกาสให้กับธุรกิจที่มี LMI Indicator ที่มีฐานะทางการเงินที่รายได้ต่ำกว่าปานกลาง ที่วัด LMI Indicator เป็น N ให้มีเปลี่ยนแปลงให้มากขึ้นจนวัด LMI indicator เป็น Y
2. พัฒนาแผนกลยุทธ์ในการสนับสนุนธุรกิจให้มีความหลากหลายและเหมาะสมกับธุรกิจที่มีความต้องการแตกต่างกัน








