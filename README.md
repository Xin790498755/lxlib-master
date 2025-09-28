# lxlib-master
├── app/            # 应用层
│   ├── main.c      # 主程序入口
│   ├── modules/    # 功能模块
│   │   ├── sensor/ # 传感器处理
│   │   ├── comm/   # 通信处理
│   │   └── control/ # 控制逻辑
│   └── config/     # 配置文件
├── services/       # 服务层
│   ├── task_mgr/   # 任务管理器
│   ├── event/      # 事件系统
│   └── utils/      # 工具函数
├── drivers/        # 驱动层
│   ├── bsp/        # 板级支持包
│   ├── hal/        # 硬件抽象层
│   └── periph/     # 外设驱动
├── cmake/          # cmake配置文件
├── CMakeLists      # cmake文件
└── third_party/    # 第三方库根目录