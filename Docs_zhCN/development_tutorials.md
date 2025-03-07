# 开发指南

CARLA 是开源且高度可扩展的仿真平台，用户可以根据特殊应用场景或需求创建自定义功能与内容。以下教程详细说明了如何在CARLA代码库中实现特定开发目标：

- [__版本发布__](tuto_D_make_release.md)
- [__内容升级__](tuto_D_contribute_assets.md)
- [__创建语义标签__](tuto_D_create_semantic_tags.md)
- [__创建新传感器__](tuto_D_create_sensor.md)
- [__性能基准测试__](adv_benchmarking.md)
- [__记录器文件格式__](ref_recorder_binary_file_format.md)
- [__碰撞边界生成__](tuto_D_generate_colliders.md)

## 版本发布

如需开发CARLA分支并发布代码版本，请遵循[__本指南__](tuto_D_make_release.md)。

## 内容升级

我们的资源内容存储在独立的Git LFS仓库。作为构建系统的一部分，我们会生成并上传包含最新资源内容的压缩包，并使用当前日期和提交哈希进行标记。我们会定期更新CARLA仓库中的资源包链接，具体操作请参考[__本教程__](tuto_D_contribute_assets.md)。

## 创建语义标签

CARLA已预置适用于大多数场景的语义标签集。如需添加自定义类别，请按照[__本指南__](tuto_D_create_semantic_tags.md)进行操作。

## 创建新传感器

通过修改CARLA的C++代码可以创建定制化传感器，完整指南请参见[__此处__](tuto_D_create_sensor.md)。

## 性能基准测试

CARLA提供基准测试脚本用于系统性能评估，完整说明请查看[__此文档__](adv_benchmarking.md)。

## 记录器二进制文件格式

记录器二进制文件格式的详细说明请参阅[__此文档__](ref_recorder_binary_file_format.md)。

## 生成碰撞边界

关于如何为车辆生成更精确碰撞边界的指南，请访问[__此教程__](tuto_D_generate_colliders.md)