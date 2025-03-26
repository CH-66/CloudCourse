# iOS 课表 App 开发文档

## 项目概述

这是一个iOS课表应用，允许用户导入课程表，并在灵动岛（Dynamic Island）和锁屏界面显示当前课程或当天的课表信息。应用采用SwiftUI构建UI，支持深色模式，并遵循Apple设计规范。

## 技术方案

### 开发语言与框架
- **编程语言**：Swift
- **UI框架**：SwiftUI
- **扩展框架**：WidgetKit、ActivityKit (用于Live Activities)

### 核心功能模块

1. **课程数据模型**
   - 课程基本信息（名称、教师、地点、时间等）
   - 学期与周次管理
   - 数据持久化（CoreData/UserDefaults）

2. **课表导入机制**
   - 支持多种导入方式（手动添加、文件导入、教务系统API等）
   - 数据解析与验证

3. **灵动岛集成（Dynamic Island）**
   - 使用ActivityKit创建Live Activities
   - 显示当前/下一节课程信息
   - 支持紧凑型和扩展型两种显示模式

4. **锁屏组件（Lock Screen Widget）**
   - 使用WidgetKit开发锁屏小组件
   - 提供多种尺寸的组件（小、中、大）
   - 显示当天课程概览或当前课程详情

5. **主应用功能**
   - 课表周视图/日视图
   - 课程管理（添加、编辑、删除）
   - 设置与偏好

### 数据流设计

```
用户输入/导入 → 数据解析 → 数据模型 → 持久化存储 ↔ 应用/组件显示
```

## 项目结构

```
CloudCourse/
├── CloudCourse/                  # 主应用
│   ├── App/                      # 应用入口
│   ├── Models/                   # 数据模型
│   ├── Views/                    # UI视图
│   │   ├── CourseViews/          # 课程相关视图
│   │   ├── ImportViews/          # 导入相关视图
│   │   └── SettingsViews/        # 设置相关视图
│   ├── ViewModels/              # 视图模型
│   ├── Services/                # 服务层
│   │   ├── DataService/          # 数据服务
│   │   ├── ImportService/        # 导入服务
│   │   └── NotificationService/  # 通知服务
│   └── Utils/                   # 工具类
├── CourseWidgetExtension/       # 小组件扩展
│   ├── CourseWidget.swift        # 小组件实现
│   └── CourseWidgetBundle.swift  # 小组件配置
├── CourseLiveActivity/          # 灵动岛扩展
│   ├── CourseLiveActivity.swift  # 灵动岛活动实现
│   └── CourseActivityAttributes.swift # 活动属性定义
└── Shared/                      # 共享代码
    ├── Models/                   # 共享数据模型
    └── Utils/                    # 共享工具类
```

## UI设计建议

### 设计原则
- 遵循Apple Human Interface Guidelines
- 简洁直观的信息展示
- 支持浅色/深色模式
- 适配不同屏幕尺寸

### 主要界面

1. **课表视图**
   - 网格式周视图，显示一周课程
   - 日视图，显示当天详细课程
   - 使用颜色区分不同课程类型
   - 支持手势操作（滑动切换周次等）

2. **课程详情**
   - 显示课程完整信息
   - 提供快速操作（添加提醒、分享等）

3. **导入界面**
   - 步骤引导式UI
   - 导入进度与结果反馈

4. **灵动岛与锁屏组件**
   - 灵动岛：紧凑型显示课程名称和时间，扩展型增加教室和教师信息
   - 锁屏小组件：根据尺寸显示不同详细程度的课程信息

## 启动与开发

### 环境要求
- macOS Ventura或更高版本
- Xcode 14或更高版本
- iOS 16或更高版本的设备/模拟器（支持灵动岛需要iPhone 14 Pro或更新机型）

### 启动命令
1. 克隆项目仓库
2. 使用Xcode打开`CloudCourse.xcodeproj`
3. 选择目标设备（推荐使用支持灵动岛的真机测试）
4. 点击运行按钮或使用快捷键`Cmd+R`

## 后续开发计划

1. 基础框架搭建
2. 数据模型设计与实现
3. 主应用UI开发
4. 课表导入功能实现
5. 小组件开发
6. 灵动岛集成
7. 测试与优化
8. 发布准备