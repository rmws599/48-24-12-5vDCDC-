# 48V转5V 多级DCDC降压电源模块设计

## 📌 项目简介
本项目设计了一款宽电压输入、多级降压的电源板，采用 **48V → 24V → 12V → 5V** 的“电源树”级联架构。从零独立完成了选型、原理图绘制、四层PCB Layout、BOM核对及打样全流程。

## 🎯 核心设计逻辑
1. **拓扑选择**：为避免单级48V转5V压差过大导致的占空比极低（D<10%）和芯片耐压超限问题，采用多级级联降压，将每一级的压差控制在合理范围。
2. **芯片选型与权衡**：
   - **U1 (LM5145)**：48V转24V，选用耐压100V的控制器，应对高压输入。
   - **U2 (TPS5430)**：24V转12V，选用经典集成管芯片，满足约1.8A电流，极致降本。
   - **U3 (LM5148)**：12V转5V，目标输出20W（4A大电流），选用外挂MOS方案，保证散热与效率。
3. **功率预算倒推**：基于90%的转换效率，从目标输出20W倒推前级功率，确保前一级有足够余量喂饱后一级。

## 📐 设计成果展示

### 1. 设计草稿：电源树及功率预算
![电源树]/><img width="828" height="1241" alt="电源树" src="https://github.com/user-attachments/assets/1c79919c-c2c1-46f6-a215-e7f97d8f3f57" />

### 2. 原理图设计
![原理图]
<img width="2313" height="1503" alt="原理图1" src="https://github.com/user-attachments/assets/bb681f99-7912-4af3-b89b-71d485941dd2" />
<img width="2235" height="1438" alt="原理图2" src="https://github.com/user-attachments/assets/c690d80c-4f28-4e86-9f83-b80aa11ecc72" />

### 3. PCB Layout（四层板）
![PCB布局]
<img width="634" height="382" alt="pcb" src="https://github.com/user-attachments/assets/574a5a2a-36a7-4ede-bf58-84497a7f3842" />
<img width="650" height="375" alt="pcb2" src="https://github.com/user-attachments/assets/dbdcf42a-db41-412c-8534-630dc34a62e7" />

### 4. BOM及物料选型
![BOM表]<img width="1338" height="651" alt="bom表" src="https://github.com/user-attachments/assets/fe130b2f-cb5c-4864-a2eb-1b1ad5c848fb" />


*(注：实物打样及上电测试照片，将于焊接测试完成后更新)*

## ⚠️ 踩坑与复盘记录（核心实战经验）
- **电容耐压降额**：最初选用16V的Cvcc电容，意识到12V输入下耐压余量不足（仅为1.3倍），果断更换为25V，深刻理解了硬件降额设计的重要性。
- **四层板DRC排查**：在布线时遇到“过孔换层无法连接目标焊盘”的报错。通过调整局部布局，在目标焊盘附近增加过孔回流路径，最终实现DRC清零。
- **BOM与供应链优化**：官方计算器推荐的元器件极端且需订货。结合实际供应链，放宽参数选取高性价比现货电感，有效控制了打样成本。

## 👤 关于作者
- 目前正在寻找 **PCB助理工程师 / 硬件助理工程师** 岗位。
- 具备嘉立创EDA、AD软件基础，熟悉焊接、万用表调试及完整打样流程。
- 期待能有一个踏实积累技术的平台，与公司共同成长！
