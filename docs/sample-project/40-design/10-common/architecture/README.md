# Architecture Design Area

`architecture/` giữ các structural decisions ở mức cao. Không dùng một `APP-ARCH-*` duy nhất để chứa mọi view khi project lớn.

Recommended decomposition:

```text
architecture/
├── APP-ARCH-001.md        # overview / architecture principles
├── system/                # systems/subsystems/components and responsibilities
├── module/                # application/module boundaries and dependency direction
└── runtime/               # runtime processes/services and interaction topology
```

## System Architecture
Quyết định solution gồm những major component/subsystem nào, responsibility và boundary ra sao.

## Module Architecture
Quyết định code/application modules, dependency rules, ownership và public/private boundaries.

## Runtime Architecture
Quyết định runtime units, process/service interaction, synchronous/asynchronous paths và runtime dependencies.

Physical deployment/network/storage topology thuộc `../infrastructure/`, không trộn vào logical/module architecture.