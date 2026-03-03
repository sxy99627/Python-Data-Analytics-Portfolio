# **酒店预订需求分析项目**
## **项目概述**
本项目基于 Kaggle 公开的酒店预订需求数据集，通过数据清洗、探索性分析和可视化，深入理解城市酒店与度假酒店的预订行为规律、季节性趋势、价格分布特征以及客户取消预订的关键影响因素。最终为酒店运营策略（如动态定价、营销时机、取消率控制）提供数据驱动的建议。
## **数据说明**
来源：Kaggle（Hotel booking demand）
数据量：119,390条记录，32个字段，主要字段解释如下：
- hotel：酒店类型（City Hotel / Resort Hotel）
- is_canceled：是否取消（1=取消，0=未取消）
- lead_time：入住时间 从预订日期到实际入住日期的间隔天数
- arrival_date_*：入住日期相关字段（年、月、周、日）
- stays_in_weekend_nights / stays_in_week_nights：周末和工作日住宿晚数
- adults / children / babies：入住人数（成人、儿童、婴儿） 
- meal：订餐情况（BB/HB/FB/SC）
- country：客源国家
- market_segment / distribution_channel：市场细分和分销渠道
- is_repeated_guest：是否回头客
- previous_cancellations / previous_bookings_not_canceled：历史取消和成功入住次数
- reserved_room_type / assigned_room_type：预订房型和实际分配房型
- booking_changes：预订更改次数
- deposit_type：押金类型
- agent / company：预订代理ID和公司 
- days_in_waiting_list：确认订单前的审核天数
- customer_type：客户类型
- adr：平均每日房价
- required_car_parking_spaces：所需停车位数量
- total_of_special_requests：特殊请求总数
- reservation_status / reservation_status_date：最终预订状态及日期
## **分析流程**
### **1. 数据清洗**
**处理缺失值**：
children：4 条缺失，用中位数填充。
country：488 条缺失，用众数填充。
agent：16340 条缺失，用 0 填充（表示无代理）。
company：缺失率 94.3%，直接删除该列。
**处理不一致数据**：
将 meal 中的 "Undefined" 统一替换为 "SC"。
将children、agent修改数据类型。
**处理无效数据**：
删除 adults+children+babies = 0 的行。
删除 adr > 5000 的离群值。
**衍生新特征**：
合并 arrival_date_year、arrival_month、arrival_date_day_of_month 生成 arrival_date 日期列。
计算总住宿晚数 stays_nights_total = stays_in_weekend_nights + stays_in_week_nights。
计算总人数 number_of_people = adults + children + babies。
### **2. 探索性分析**
**基本指标**：
城市酒店预订量占 66.4%，度假酒店占 33.6%。
总体入住率 62.92%，取消率 37.08%。
度假酒店取消率 27.8%，城市酒店取消率 41.8%。
总复定率仅 3.15%，度假酒店略高于城市酒店。
**房型分析**：
房型 A 和 D 入住率最高，是主流房型；度假酒店的 E 型也有较高入住率。
**季节性分析**：
城市酒店夏季入住高峰，冬季最低；度假酒店四季相对平稳。
**入住晚数分布**：
主要集中在 1-3 晚；度假酒店 7 晚预订也较常见，偏好房型 A、D、E。
**取消因素分析**：
- 房型不一致对取消影响很小（仅占取消订单 1%）。
- 餐型分布与取消无关。
- 分销渠道中 "TA/TO" 渠道退订占比高达 41.1%，明显高于其他渠道。
- 无定金预订更受欢迎，但仍有大量预付定金订单取消。
- 提前预定时长与取消率呈正相关。一般越早预定，也越容易取消。
- 与取消率相关性较高的因素包括：
lead_time、total_of_special_requests、required_car_parking_spaces、booking_changes、previous_cancellations。
## **主要发现与建议**
1.预订量与取消率：城市酒店预订量大但取消率高，建议优化渠道服务，增加“附近优选”功能，提升用户体验。
2.客户粘性：复定率极低，需梳理服务流程，提升质量，并通过多渠道增加曝光。
3.房型优化：主推 A、D 房型，适当调整其他低入住率房型比例；度假酒店可保留特色 E 型。
4.季节性策略：城市酒店可在夏季加大营销，冬季推出优惠；度假酒店可保持平稳定价。
5.取消风险管理：
- 重点关注 TA/TO 渠道，核查分销奖励机制。
- 对提前较长时间预定的客户进行再确认，并考虑推出“早鸟优惠”锁定订单。
- 针对历史取消次数多、提出特殊要求多的客户，可设置预警或加强跟进。
## **技术栈**
- Python 
- Pandas、NumPy（数据处理）
- Matplotlib、Seaborn（可视化）
- Jupyter Notebook（开发环境）
