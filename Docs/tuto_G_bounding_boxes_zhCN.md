# 边界框

## 设置模拟器
```python
settings.synchronous_mode = True # 启用同步模式
settings.fixed_delta_seconds = 0.05
world.apply_settings(settings)

# 创建队列存储传感器数据
image_queue = queue.Queue()
camera.listen(image_queue.put)
```

## 几何变换
```python
# 构建世界坐标系到相机坐标系的转换矩阵
world_2_camera = np.array(camera.get_transform().get_inverse_matrix())

# 获取相机属性
image_w = camera_bp.get_attribute("image_size_x").as_int()
image_h = camera_bp.get_attribute("image_size_y").as_int()
fov = camera_bp.get_attribute("fov").as_float()

# 计算3D→2D投影矩阵
K = build_projection_matrix(image_w, image_h, fov)
```

## 导出边界框
### Pascal VOC格式
```xml
<annotation>
    <folder>output</folder>
    <filename>023235.png</filename>
    <size>
        <width>800</width>
        <height>600</height>
        <depth>3</depth>
    </size>
    <object>
        <name>vehicle</name>
        <bndbox>
            <xmin>503</xmin>
            <ymin>310</ymin>
            <xmax>511</xmax>
            <ymax>321</ymax>
        </bndbox>
    </object>
</annotation>
```

### COCO格式
```python
simulation_dataset = {
    "images": [{
        "file_name": "023235.png",
        "height": 600,
        "width": 800,
        "id": 23235
    }],
    "annotations": [{
        "image_id": 23235,
        "category_id": 1,
        "bbox": [503, 310, 8, 11]
    }]
}
```

*注意：本教程未处理边界框重叠情况，实际应用中需添加前景框识别逻辑*