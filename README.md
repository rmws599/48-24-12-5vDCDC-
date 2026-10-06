# 48V转5V 多级DCDC降压电源模块设计（含硬件Debug与DFM复盘）

## 📌 项目简介
采用 48V → 24V → 12V → 5V 级联架构的降压电源板。独立完成选型、原理图、四层PCB、BOM、打样、焊接与上电调试，并在调试中定位了DFM工艺缺陷与电路逻辑错误，最终在嘉立创与AD双版本中完成DRC 0错误全绿。

## 🎯 核心设计逻辑
1. **拓扑选择**：为避免单级48V转5V压差过大导致的占空比极低（D<10%）和芯片耐压超限问题，采用多级级联降压，将每一级的压差控制在合理范围。
2. **芯片选型与权衡**：
   - **U1 (LM5145)**：48V转24V，选用耐压100V的控制器，应对高压输入。
   - **U2 (TPS5430)**：24V转12V，选用经典集成管芯片，满足约1.8A电流，极致降本。
   - **U3 (LM5148)**：12V转5V，目标输出20W（4A大电流），选用外挂MOS方案，保证散热与效率。
3. **功率预算倒推**：基于90%的转换效率，从目标输出20W倒推前级功率，确保前一级有足够余量喂饱后一级。

## ⚠️ 踩坑与复盘记录

### 🛠️ 一、DFM极限短路排查
空板实测输入端阻抗仅 0Ω，排查后确认为 安全间距误设为0mil，导致48V过孔与内层GND在板厂压合时物理短接。教训：DRC通过不等于工厂能造出来。

### 🚨 二、 CVIN1电容逻辑修正
排查中发现第一级去耦电容CVIN1两端被误接入同一网络（VIN_48V），导致滤波功能完全失效。V2.0已修正为标准的“一端接VIN引脚，一端接GND”。

### ✅ 三、 AD版AGND/PGND单点接地优化
嘉立创版本无法区分模拟地与功率地。AD重画版中，第一级与第三级芯片的AGND分别通过一个0Ω电阻就近连接至PGND（第二级芯片本身未区分），有效降低开关噪声对反馈环路的干扰。

### ✅ 四、 最终优化
安全间距修正为8mil，删除高压区冗余过孔，最终实现 嘉立创EDA全量DRC检查（124项）0错误全绿。


## 📐 设计成果展示

### 1. 设计草稿：电源树及功率预算
![电源树]/><img width="828" height="1241" alt="电源树" src="https://github.com/user-attachments/assets/1c79919c-c2c1-46f6-a215-e7f97d8f3f57" />

### 2. 原理图设计
嘉立创版V2.0：
<img width="2312" height="1502" alt="V2 0嘉立创原理图" src="https://github.com/user-attachments/assets/799a5c80-a227-400c-a98a-841f4b705242" />
<img width="2235" height="1438" alt="原理图2" src="https://github.com/user-attachments/assets/72e1d54d-c24e-4389-ba39-4b19e0b0de1c" />
AD版V2.0：
<img width="1100" height="763" alt="V2 0_AD原理图1" src="https://github.com/user-attachments/assets/24037aff-20f2-4802-a8bf-d822ab95fdfb" />
<img width="1050" height="742" alt="V2 0_AD原理图2" src="https://github.com/user-attachments/assets/7f7a71d6-f6e4-47cd-9d6e-7aec769f85ab" />


### 3. PCB Layout（四层板）
嘉立创V2.0版：
<img width="1080" height="636" alt="v2 0嘉立创pcb" src="https://github.com/user-attachments/assets/93702a25-d6c1-4e53-973a-82287c3cf156" />
<img width="650" height="375" alt="pcb2" src="https://github.com/user-attachments/assets/19f4e950-3dda-4a9e-8f8b-1a8f34932cea" />

AD版V2.0:
<img width="1293" height="758" alt="v2 0_ADpcb" src="https://github.com/user-attachments/assets/741fd52b-6f56-431b-b90a-1ee77870e2ff" />
<img width="1275" height="740" alt="V2 0_ADPCB2" src="https://github.com/user-attachments/assets/a9c47e85-3c80-4cb6-8913-7ca70e11030c" />

### 4. 0Ω物理短路实测
<img width="1279" height="1704" alt="0Ω" src="https://github.com/user-attachments/assets/43236dbb-9da2-44b5-9259-9550da16427f" />


### 5. BOM及物料选型
![BOM表]<img width="1338" height="651" alt="bom表" src="https://github.com/user-attachments/assets/fe130b2f-cb5c-4864-a2eb-1b1ad5c848fb" />

### 6. 最终迭代：DRC检查0错误全绿
<img width="1411" height="815" alt="DRC" src="https://github.com/user-attachments/assets/0e566d05-36ad-4a17-ae97-89b0fe0df52d" />

*(注：实物打样及上电测试照片，将于焊接测试完成后更新)*

## 👤 关于作者
- 目前正在寻找 **PCB助理工程师 / 硬件助理工程师** 岗位。
- 具备嘉立创EDA、AD软件基础，熟悉焊接、万用表调试及完整打样流程。
- 期待能有一个踏实积累技术的平台，与公司共同成长！
