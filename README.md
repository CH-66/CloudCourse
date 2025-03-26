# CloudCourse - iOS 课表应用

## 项目概述

CloudCourse 是一款 iOS 课表应用，允许用户导入课程表，并在灵动岛（Dynamic Island）和锁屏界面显示当前课程或当天的课表信息。应用采用 SwiftUI 构建用户界面，遵循 Apple Design Guidelines，并支持深色模式。

## 技术栈

- **编程语言**：Swift
- **UI 框架**：SwiftUI
- **系统集成**：Live Activities、WidgetKit
- **最低支持系统**：iOS 16.0+（支持灵动岛需要 iPhone 14 Pro 及以上机型）

## 项目结构

```
CloudCourse/
├── CloudCourse/                  # 主应用代码
│   ├── App/                      # 应用入口
│   ├── Models/                   # 数据模型
│   ├── Views/                    # UI 视图
│   │   ├── Course/              # 课程相关视图
│   │   ├── Schedule/            # 课表相关视图
│   │   ├── Settings/            # 设置相关视图
│   │   └── Components/          # 可复用组件
│   ├── ViewModels/              # 视图模型
│   ├── Services/                # 服务层
│   │   ├── DataService/         # 数据服务
│   │   ├── ImportService/       # 导入服务
│   │   └── NotificationService/ # 通知服务
│   ├── Utils/                   # 工具类
│   └── Resources/               # 资源文件
├── CloudCourseWidget/           # 小组件扩展
├── CloudCourseLiveActivity/     # 灵动岛和锁屏活动
└── doc/                         # 项目文档
    ├── README.md                # 项目说明
    ├── DevelopmentProgress.md   # 开发进度
    └── DataDictionary.md        # 数据字典
```

## 技术方案

### 1. 数据模型设计

应用将使用 Core Data 作为本地数据存储方案，主要数据模型包括：

- **Course**：课程信息，包含课程名称、教师、教室等
- **Schedule**：课表信息，包含周几、开始时间、结束时间等
- **Semester**：学期信息，用于区分不同学期的课表

### 2. 课表导入方案

支持多种导入方式：

- 手动添加课程
- 从 Excel/CSV 文件导入
- 支持主流教务系统导出格式
- 通过 URL Scheme 从其他应用导入

### 3. 灵动岛和锁屏组件实现

使用 WidgetKit 和 ActivityKit 框架实现：

- **Live Activities**：显示当前正在进行的课程，包括课程名称、教室、剩余时间等
- **锁屏小组件**：显示今日课程列表或下一节课信息
- **灵动岛**：课程开始前通知，课程进行中显示进度

### 4. UI 设计方案

- 遵循 Apple Human Interface Guidelines
- 支持浅色/深色模式自动切换
- 采用卡片式设计展示课程信息
- 使用日历视图和列表视图双模式展示课表
- 支持自定义主题色和课程颜色标记

## UI 设计建议

1. **主界面**：采用日历+列表的组合视图，顶部为周视图日历，下方为当日课程列表
2. **课程卡片**：使用圆角矩形卡片，显示课程名称、时间、地点，并用不同颜色区分不同课程
3. **灵动岛**：紧凑模式显示课程名称和剩余时间，展开模式额外显示教室位置和教师信息
4. **锁屏组件**：提供多种尺寸的小组件，小尺寸显示下一节课，中尺寸显示当天课程列表
5. **设置界面**：提供主题设置、通知设置、导入/导出设置等选项

## 开发环境与启动命令

### 开发环境

- Xcode 14.0+
- iOS 16.0+ SDK
- Swift 5.7+
- macOS 12.0+

### 启动命令

1. 克隆项目到本地：
   ```bash
   git clone https://github.com/yourusername/CloudCourse.git
   ```

2. 打开项目：
   ```bash
   cd CloudCourse
   open CloudCourse.xcodeproj
   ```

3. 在 Xcode 中选择目标设备并运行

## 后续开发计划

1. 实现基础课表管理功能
2. 开发课表导入功能
3. 实现灵动岛和锁屏组件
4. 添加自定义主题和设置
5. 优化用户体验和性能
6. 添加云同步功能（可选）