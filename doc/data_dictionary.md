# iOS 课表 App 数据字典

## 文档概述

本文档详细定义了iOS课表应用中的所有数据结构和字段，包括课程、课程表、学期等核心实体的属性和关系。该数据字典为开发团队提供了清晰的数据模型参考，确保在开发过程中数据结构的一致性和完整性。

## 核心数据模型

### 1. Course（课程）

课程是应用的核心实体，代表用户的每一门课程。

| 字段名 | 类型 | 必填 | 描述 |
|-------|------|------|------|
| id | UUID | 是 | 课程唯一标识符 |
| name | String | 是 | 课程名称 |
| teacher | String | 否 | 授课教师 |
| location | String | 否 | 上课地点 |
| courseCode | String | 否 | 课程代码 |
| credit | Float | 否 | 学分 |
| courseType | CourseType | 否 | 课程类型（枚举） |
| color | Color | 否 | 课程在UI中显示的颜色 |
| notes | String | 否 | 课程备注 |
| semesterId | UUID | 是 | 关联的学期ID |

### 2. CourseSchedule（课程安排）

课程安排定义了课程的具体时间安排，一门课程可能有多个课程安排（如周一和周三各一次课）。

| 字段名 | 类型 | 必填 | 描述 |
|-------|------|------|------|
| id | UUID | 是 | 安排唯一标识符 |
| courseId | UUID | 是 | 关联的课程ID |
| weekday | Int | 是 | 星期几（1-7，1代表周一） |
| startTime | Time | 是 | 上课开始时间 |
| endTime | Time | 是 | 上课结束时间 |
| startWeek | Int | 是 | 开始于第几周 |
| endWeek | Int | 是 | 结束于第几周 |
| weekType | WeekType | 否 | 周类型（每周、单周、双周） |
| location | String | 否 | 特定安排的上课地点（可覆盖课程默认地点） |

### 3. Semester（学期）

学期是课程的组织单位，定义了学期的起止时间和周数。

| 字段名 | 类型 | 必填 | 描述 |
|-------|------|------|------|
| id | UUID | 是 | 学期唯一标识符 |
| name | String | 是 | 学期名称（如"2023-2024学年第一学期"） |
| startDate | Date | 是 | 学期开始日期 |
| endDate | Date | 是 | 学期结束日期 |
| totalWeeks | Int | 是 | 总周数 |
| isActive | Bool | 是 | 是否为当前活跃学期 |

### 4. TimeSlot（时间段）

时间段定义了学校的作息时间，用于标准化课程的开始和结束时间。

| 字段名 | 类型 | 必填 | 描述 |
|-------|------|------|------|
| id | UUID | 是 | 时间段唯一标识符 |
| name | String | 是 | 时间段名称（如"第一节"） |
| startTime | Time | 是 | 开始时间 |
| endTime | Time | 是 | 结束时间 |
| order | Int | 是 | 排序顺序 |
| semesterId | UUID | 是 | 关联的学期ID（不同学期可能有不同的作息时间） |

### 5. Notification（通知）

通知定义了课程的提醒设置。

| 字段名 | 类型 | 必填 | 描述 |
|-------|------|------|------|
| id | UUID | 是 | 通知唯一标识符 |
| courseScheduleId | UUID | 是 | 关联的课程安排ID |
| minutesBefore | Int | 是 | 提前多少分钟通知 |
| isEnabled | Bool | 是 | 是否启用 |
| notificationType | NotificationType | 是 | 通知类型（枚举） |

## 枚举类型

### 1. CourseType（课程类型）

```swift
enum CourseType: String, Codable, CaseIterable {
    case required = "必修"
    case elective = "选修"
    case optional = "任选"
    case physical = "体育"
    case lab = "实验"
    case other = "其他"
}
```

### 2. WeekType（周类型）

```swift
enum WeekType: String, Codable, CaseIterable {
    case all = "每周"
    case odd = "单周"
    case even = "双周"
}
```

### 3. NotificationType（通知类型）

```swift
enum NotificationType: String, Codable, CaseIterable {
    case inApp = "应用内通知"
    case system = "系统通知"
    case both = "两者都有"
}
```

## 数据关系

### 实体关系图

```
Semester (1) ----< Course (1) ----< CourseSchedule (1) ----< Notification (N)
    |
    ----< TimeSlot (N)
```

### 关系说明

1. 一个学期（Semester）包含多个课程（Course）
2. 一个课程（Course）包含多个课程安排（CourseSchedule）
3. 一个课程安排（CourseSchedule）可以有多个通知（Notification）
4. 一个学期（Semester）定义了多个时间段（TimeSlot）

## 持久化策略

### CoreData 模型

应用将使用CoreData作为主要的持久化方案，所有核心实体将被映射为CoreData实体。

### 数据迁移策略

1. **轻量级迁移**：对于简单的模型变更（如添加属性、重命名属性等），使用CoreData的轻量级迁移。
2. **映射模型迁移**：对于复杂的模型变更（如实体关系变更、属性类型变更等），使用映射模型进行迁移。
3. **版本控制**：每次模型变更时，增加模型版本号，并保留旧版本模型以支持向后兼容。

### 缓存策略

1. **内存缓存**：频繁访问的数据（如当前学期的课程）将被缓存在内存中，以提高访问速度。
2. **预加载**：应用启动时，预加载当前学期的课程数据，减少用户等待时间。
3. **懒加载**：非当前学期的数据采用懒加载策略，仅在需要时从数据库加载。

## 数据导入/导出

### 导入格式支持

1. **手动输入**：通过应用界面手动添加课程信息。
2. **Excel/CSV导入**：支持从Excel或CSV文件导入课程数据。
3. **教务系统API**：支持从学校教务系统API导入课程数据（如适用）。

### 导出格式

1. **JSON格式**：支持将课程数据导出为JSON格式，便于备份和共享。
2. **iCalendar格式**：支持将课程导出为iCalendar格式，便于与其他日历应用集成。

## 数据安全

1. **本地存储**：所有数据默认仅存储在用户设备本地，不上传到云端。
2. **备份支持**：支持通过iCloud备份应用数据。
3. **数据加密**：敏感数据（如教务系统账号密码）将使用iOS的安全存储机制（如Keychain）进行加密存储。

---
