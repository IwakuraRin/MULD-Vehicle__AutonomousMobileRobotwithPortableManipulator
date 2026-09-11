# SMT 桌面贴片机

软件、通信协议和机械设计放在同一个 Git 仓库中，共同维护版本。当前为工程骨架，尚未包含可运行的控制程序或固件。

## 项目结构

```text
SMT/
├── console/              # 笔记本浏览器界面
├── controller/           # RK3566：设备流程、视觉、通信及 Web 服务
├── firmware/
│   └── mainboard/        # 下位机固件，芯片和工具链待确定
├── protocols/            # 网页 API、下位机协议及版本约定
├── hardware/
│   ├── mechanical/
│   │   ├── source/       # 可编辑的机械 CAD 源工程
│   │   ├── step/         # STEP/STP 三维交换文件
│   │   └── drawings/     # DWG/DXF 图纸及配套 PDF
│   └── electronics/      # 电路原理图、PCB、BOM
├── config/
│   └── examples/         # 可公开提交的配置示例
├── docs/                 # 架构、标定和部署文档
├── tests/
│   └── fixtures/         # 小型测试图片、协议报文等固定样例
└── tools/                # 构建、部署和开发辅助脚本
```

## 运行方式

- 笔记本负责开发和浏览器操作界面；界面构建产物部署到 RK3566，由板端提供网页。
- RK3566 使用适配该板的 Linux，运行控制服务和视觉程序。
- 下位机固件独立编译和烧录，负责实时运动执行和硬件保护。
- STEP/DWG 等机械文件由 CAD 软件打开，不参与软件构建。

同一个仓库不代表同一个进程、编译器或部署包。各部分保留独立的依赖和构建入口，在协议层约定协作方式。

第一版计划采用 Python、OpenCV 和 FastAPI 实现板端服务。网页框架、下位机芯片和通信接口确定后，再补充对应工程配置。

## 文件管理

- 可编辑 CAD 源工程放 `hardware/mechanical/source/`，交换模型放 `step/`，工程图放 `drawings/`。
- 图纸和软件之间存在依赖时，在同一次提交中更新相关文件与说明；发布时记录对应的硬件版本。
- 使用稳定文件名，例如 `head-assembly.step`、`camera-bracket.dwg`；历史修订交给 Git 管理。
- 板厂 SDK、系统镜像、编译产物、完整录像和日常采集图片不直接提交。SDK 下载地址、版本及校验值记录在文档中。
- 本机 SDK 放根目录 `sdk/`，工具链放 `toolchains/`，系统镜像放 `system-images/`，发布构建产物放 `artifacts/`；这些目录均已忽略。不要把它们混放在源码目录中。
- 设备实际标定和运行数据放根目录 `local/`，该目录已忽略；需要另行备份。可复现的配置样例放 `config/examples/`。
- 本机密钥或凭据放 `secrets/` 或本机 `.env`，均不提交；`.env.example` 只放占位值。正式图纸、依赖锁文件、共享编辑器配置和必要的预编译库应提交。
- 当前未启用 Git LFS。大型 CAD 文件正式导入前，建议先安装 Git LFS、确认远程仓库支持及容量，再提交对应跟踪规则。不要等历史中积累大量图纸后再迁移。

## 下一步

1. 确认 RK3566 板型号、Linux 镜像和 USB 相机接口能力。
2. 确认下位机芯片、通信接口和已有固件。
3. 在 `protocols/` 定义最小控制协议，再实现模拟下位机。
4. 跑通网页点动、相机预览、标定和单次取放。

架构约定见 [docs/architecture.md](docs/architecture.md)。
