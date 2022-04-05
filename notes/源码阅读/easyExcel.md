## EasyExcel
简化`Excel`操作，整体的设计与方案。
如：更加友好的API：链式调用，数据与操作读写的分离
```text
ExcelWriterBuilder  --  ExcelWriterSheetBuilder
创建文件(文件名) -- 表头数据 -- Sheet页名称 -- 具体的内容数据
EasyExcel.write(fileName, DemoData.class).sheet("模板").doWrite(this::data);
```
### 设计模式的是使用
- 工厂模式
> `EasyExcelFactory`
![UML类图](https://i.bmp.ovh/imgs/2021/12/c60fae06ba0ed4de.png)