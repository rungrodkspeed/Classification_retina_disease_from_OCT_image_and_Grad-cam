### การจำแนกโรคจอประสาทตาด้วยรูปภาพ OCT และการใช้ Grad-cam ในการประเมินความรุนแรงของอาการ
-------------
ดวงตาเป็นอวัยวะสำคัญในการใช้ชีวิตของมนุษย์ หากเกิดความเสียหายของดวงตาขึ้นหรือสูญเสียการมองเห็นไปก็จะทำให้การใช้ชีวิตลำบากขึ้นอย่างมาก และเนื่องจากประชากรที่ป่วยเป็นโรคเกี่ยวกับดวงตามากขึ้น แต่จักษุแพทย์ที่เป็นผู้เชี่ยวชาญนั้นกลับมีน้อยมาก ดังนั้นหากเราสามารถสร้างเครื่องมือที่สามารถประเมินอาการของโรคเบื้องต้นได้และสามารถทำให้เห็นถึงสาเหตุของโรคได้ชัดขึ้น ก็จะทำให้แพทย์ทำง่ายได้ดียิ่งขึ้นและเร็วมากขึ้น ส่งผลให้การรักษาผู้ป่วยเป็นไปอย่างมีประสิทธิภาพและรู้ถึงสาเหตุของอาการได้เร็วขึ้น


### Data
-------------
โดยชุดข้อมูลที่เรานำมาใช้สร้างเครืองมือชื่อว่า Labeled Optical Coherence Tomography (OCT) and Chest X-Ray Images for Classification , version 2, เผยแพร่วันที่ 6 มกราคม 2018 (ปัจจุบันมี version 3 แล้ว) จาก https://data.mendeley.com/datasets/rscbjbr9sj/2
ซึ่งเราจะใช้ชุดข้อมูลที่เป็นภาพ OCT เท่านั้น โดยภายในชุดข้อมูลจะประกอบไปด้วย 3 folders คือ train, test และ val แล้วในแต่ละ folder นั้นจะมีไฟล์ภาพ OCT ที่มีลักษณะที่บ่งบอกถึงโรคต่างๆ คือ CNV(Choroidal neovasclarization), DME(Diabetic macular edema), Drusen และ Normal ดังภาพ

<p align="center">
  <img src="/blob/data.png" />
</p>

### Preprocessing Data
-------------
- ใช้ Z-scores Normalization ในการ nomalize ภาพแต่ละไฟล์
- ใช้ Data Augumentation เพื่อให้ตัวโมเดล robust ต่อ noise ต่างๆ โดยใช้หลักการ flip, shift, rotate และการ zoom 

และเมื่อเราทำการประมวลผลเสร็จแล้วจะได้รูปร่างหน้าตาของข้อมูลดังภาพ

<p align="center">
  <img src="/blob/preprocessing.png" />
</p>


### Model
-------------
ในส่วนการสร้างโมเดลเพื่อให้เครื่องคอมรู้จำได้นั้น เราจะใช้ Convolution neural network แบบ densenet121 

<p align="center">
  <img src="/blob/densenet121.png" />
</p>
<p align="center">
  ภาพจาก: https://pytorch.org/hub/pytorch_vision_densenet/
</p>
<br />
- ผลลัพธ์จากการเทรน (Accuracy)


<p align="center">
  <img src="/blob/accuracy.png" />
</p>

<br />
- ผลลัพธ์จากการเทรน (Loss)

<p align="center">
  <img src="/blob/loss.png" />
</p>

### Result
-------------
เมื่อนำโมเดลดังกล่าวไปทดสอบแล้วจะได้ผลลัพธ์ดังนี้

<p align="center">
  <img src="/blob/cm.png" />
</p>

### Grad-cam
-------------

Grad-cam คือการ visualize สิ่งที่โมเดลเห็นจาก features ที่มีลักษณะสำคัญในการ classification ของโมเดลที่เราได้สร้างเอาไว้โดยใช้ heatmap 

ซึ่งหลังจากเราเทรน model เสร็จแล้ว เราสามารถย้อนหาบริเวณที่ต้องการได้จากการนำเอา output ในชั้นสุดท้ายที่เป็น feature map สองมิติในแต่ละ channel มาคูณเข้ากับค่า weight ก็จะได้ในส่วนของ heatmap ออกมา

โดยกำหนดภาพ input คือ

<p align="center">
  <img src="/blob/input.jfif" />
</p>

ภาพ heatmap ที่ได้ออกมา

<p align="center">
  <img src="/blob/heatmap.png" />
</p>

หลังจากที่ได้ heatmap ออกมาแล้ว เราจะนำภาพ input + heatmap ก็จะได้บริเวณที่คาดว่าเป็นสาเหตุของโรคนั้นๆ ดังภาพ

<p align="center">
  <img src="/blob/im_out.jpg" />
</p>
