## Prediction of stock price direction
โปรเจ็คนี้เป็นการสร้างโมเดลทำนายราคาหุ้นที่คาดว่าจะซื้อหรือขาย (ราคาปิดของ Period ข้างหน้ามากกว่า Period ปัจจุบัน โดย Period ปัจจุบันจะถูกบันทึกว่าต้องซื้อ(1) ) ในทุกๆ 15 นาที กับหุ้น 3 ตัว ได้แก่ ERW, TISCO, SPRC โดยดูจาก feature ข้อมูลการเปลี่ยนแปลงราคาในรอบ 15 นาที ที่สูงที่สุดที่โมเดลทำนายได้ของแต่ละหุ้น โดยข้อมูลที่ใช้ในการ train และ test โมเดลเป็นข้อมูลราคาหุ้น ในอดีตย้อนหลัง ประมาณ 60 วันโดยประมาณ ตั้งแต่ช่วงวันที่ทำการรันข้อมูลหักลบย้อนกลับไป 60 วัน

โดยมีขั้นตอนดังต่อไปนี้
<img width="836" alt="ภาพถ่ายหน้าจอ 2567-01-14 เวลา 09 34 06" src="https://github.com/Taralimz/DADS-6003-Final-project/assets/122988569/6fed83f3-88d0-4d3e-acbd-d84c41b1889f">
1. โหลดข้อมูล(Collect Data): นำเข้าชุดข้อมูลที่ได้รับการปรับปรุงและการเลือกคุณลักษณt
2. แบ่งข้อมูลเป็นชุดฝึกและทดสอบ: ใช้ TimeSeriesSplit แบ่งเป็น 80% สำหรับการฝึก, 20% สำหรับการทดสอบ
3. ฝึกโมเดลด้วย Cross-Validation: วนลูปผ่านแต่ละโมเดล:
  - LGBM
  - Gradient Boosting
  - Random Forest
  - Logistic Regression (L1, L2, Elasticnet)
  - GaussianNB
  - SVM
โดยแต่ละโมเดลจะนำ CrossValidation TimeSeriesSplit มาใช้เพื่อป้องกันการ overfitting
ฝึกโมเดลในช่วงการฝึกประเมิน
4. ปรับปรุงพารามิเตอร์(Hyperparameter Tuning) :ใช้ BayesianOptimization สำหรับการปรับปรุงพารามิเตอร์ที่มีประสิทธิภาพปรับปรุงพารามิเตอร์สำหรับแต่ละโมเดลขึ้นอยู่กับประสิทธิภาพในช่วงการทดสอบ
5. ประเมินประสิทธิภาพ(Evaluation): ทำการทำนายบนชุดทดสอบที่ถือไว้คำนวณตัวชี้ประสิทธิภาพ (เช่น accuracy, precision, recall, F1-score)
6. เลือกโมเดล:เปรียบเทียบตัวชี้ประสิทธิภาพระหว่างโมเดลและเลือกโมเดลที่ดีที่สุดตามเกณฑ์การประเมิน

## Model Selection

# ERW Model
<img width="1106" alt="ภาพถ่ายหน้าจอ 2567-01-14 เวลา 09 16 20" src="https://github.com/Taralimz/DADS-6003-Final-project/assets/122988569/6ce8aece-9001-4210-b4fb-b4ccea45e252">

  - Model : Gaussian naïve bayes
  - Feature: Stepwise_Gaussian
  - Train_Score: 0.739
  - Test_Score: 0.838
  - Avg. CV score: 0.703
  - Overfitted_ratio: 0.882

# TISCO Model
![Tisco](https://github.com/Taralimz/DADS-6003-Final-project/assets/122988569/1c574c50-5a87-4467-b55e-19668c353020)
  - Model: xgb bayesian
  - Feature: Stepwise_gradientboosting
  - Train_Score: 0.785
  - Test_Score: 0.784
  - Overfitted_ratio: 1.002

  # SPRC Model
  ![SPRC](https://github.com/Taralimz/DADS-6003-Final-project/assets/122988569/252ff0f4-bd5b-496c-a6c7-8b1a3ed63eab)
  - Model: Gaussian naïve bayes
  - Feature: Stepwise_Gaussian
  - Train_Score: 0.734
  - Test_Score: 0.722
  - Overfitted_ratio: 1.017

สมาชิก
- 6520422002 อภิชัย ฟูแก้ว
- 6520422022 นริศ จันทร์ไพบูลย์กิจ
- 6520422025 ภควัต รักษาศิล


  
