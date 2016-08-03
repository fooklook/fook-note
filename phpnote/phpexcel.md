##phpexcel

###在THINKPHP中使用phpExcel

在新版本的thinkphp中引入了命名空间，而从phpExcel官网中下载下来的版本中并未引入命名空间。

解决办法如下：

- 将从[phpExcel](http://phpexcel.codeplex.com/)官网中下载下来的文件解压到ThinkPHP\Library\Vendor文件中。
- 当需要调用时通过Vendor()函数引入相关文件。

```php
Vendor('PHPExcel.PHPExcel');
$objPHPExcel = new \PHPExcel();
```

###创建一个Excel实例

```php
$objPHPExcel = new \PHPExcel();
```

###设置excel的属性

```php
//创建人
$objPHPExcel->getProperties()->setCreator("Maarten Balliauw");
//最后修改人
$objPHPExcel->getProperties()->setLastModifiedBy("Maarten Balliauw");
//标题
$objPHPExcel->getProperties()->setTitle("Office 2007 XLSX Test Document");
//题目
$objPHPExcel->getProperties()->setSubject("Office 2007 XLSX Test Document");
//描述
$objPHPExcel->getProperties()->setDescription("Test document for Office 2007 XLSX, generated using PHP classes.");
//关键字
$objPHPExcel->getProperties()->setKeywords("office 2007 openxml php");
//种类
$objPHPExcel->getProperties()->setCategory("Test result file");
//也可用下面这种方式
$objPHPExcel->getProperties()->setCreator("ctos")  
            ->setLastModifiedBy("ctos")  
            ->setTitle("Office 2007 XLSX Test Document")  
            ->setSubject("Office 2007 XLSX Test Document")  
            ->setDescription("Test document for Office 2007 XLSX, generated using PHP classes.")  
            ->setKeywords("office 2007 openxml php")  
            ->setCategory("Test result file"); 
```

###设置当前的sheet

```php
$objPHPExcel->setActiveSheetIndex(0);
```

###创建一个新的工作空间

```php
$objPHPExcel->createSheet();
$objPHPExcel->setActivesheetIndex(1);
```

###设置sheet的标题

```php
$objPHPExcel->getActiveSheet()->setTitle('Simple');
```

###设置单元格宽度

```php
$objPHPExcel->getActiveSheet()->getColumnDimension('A')->setWidth(20);
```

###设置单元格高度

```php
$objPHPExcel->getActiveSheet()->getRowDimension($i)->setRowHeight(40);
```

###合并单元格

```php
$objPHPExcel->getActiveSheet()->mergeCells('A18:E22');
```

###拆分单元格

```php
$objPHPExcel->getActiveSheet()->unmergeCells('A18:E22');
```

###设置保护cell,保护工作表

```php
$objPHPExcel->getActiveSheet()->getProtection()->setSheet(true); 
$objPHPExcel->getActiveSheet()->protectCells('A3:E13', 'PHPExcel');
```

###设置宽度

```php
//设置单元格
$objPHPExcel->getColumnDimension(‘B‘)->setAutoSize(true);  
$objPHPExcel->getColumnDimension(‘A‘)->setWidth(30);
```

###获取单元格样式

```php  
$objPHPExcel = $objActSheet->getStyle(‘A5‘);  
```

###设置单元格内容的数字格式

```php
$objPHPExcel->getActiveSheet()->getStyle('E4')->getNumberFormat()->setFormatCode(PHPExcel_Style_NumberFormat::FORMAT_CURRENCY_EUR_SIMPLE);
$objPHPExcel->getActiveSheet()->getStyle('E4')->getNumberFormat()->setFormatCode(PHPExcel_Style_NumberFormat::FORMAT_NUMBER);  
```

###复制样式

```php
$objPHPExcel->getActiveSheet()->duplicateStyle( $objPHPExcel->getActiveSheet()->getStyle('E4'), 'E5:E13' );
```

###设置加粗

```php
$objPHPExcel->getActiveSheet()->getStyle('B1')->getFont()->setBold(true);
```

###设置水平对齐方式

- HORIZONTAL_RIGHT
- HORIZONTAL_LEFT
- HORIZONTAL_CENTER
- HORIZONTAL_JUSTIFY

```php
$objPHPExcel->getActiveSheet()->getStyle('D11')->getAlignment()->setHorizontal(PHPExcel_Style_Alignment::HORIZONTAL_RIGHT);
```

###设置垂直居中

```php
$objPHPExcel->getActiveSheet()->getStyle('A18')->getAlignment()->setVertical(PHPExcel_Style_Alignment::VERTICAL_CENTER);
```

###设置字号

```php
//设置默认字体
$objPHPExcel->getActiveSheet()->getDefaultStyle()->getFont()->setSize(10);
//设置单元格字号
$objPHPExcel->getActiveSheet()->getStyle('B1')->getFont()->setSize(10);
```

###设置边框

```php
$objPHPExcel->getActiveSheet()->getStyle('A3')->getBorders()->getTop()->setBorderStyle(PHPExcel_Style_Border::BORDER_THIN);
$objPHPExcel->getActiveSheet()->getStyle('A3')->getBorders()->getTop()->getColor()->setARGB('FF000000');
$objPHPExcel->getActiveSheet()->getStyle('A3')->getBorders()->getLeft()->setBorderStyle(PHPExcel_Style_Border::BORDER_THIN);
$objPHPExcel->getActiveSheet()->getStyle('A3')->getBorders()->getLeft()->getColor()->setARGB('FF000000');
$objPHPExcel->getActiveSheet()->getStyle('A3')->getBorders()->getBottom()->setBorderStyle(PHPExcel_Style_Border::BORDER_THIN);
$objPHPExcel->getActiveSheet()->getStyle('A3')->getBorders()->getBottom()->getColor()->setARGB('FF000000');
$objPHPExcel->getActiveSheet()->getStyle('A3')->getBorders()->getRight()->setBorderStyle(PHPExcel_Style_Border::BORDER_THIN);
$objPHPExcel->getActiveSheet()->getStyle('A3')->getBorders()->getRight()->getColor()->setARGB('FF000000');   
``` 

###设置单元格背景色

```php
$objPHPExcel->getActiveSheet(0)->getStyle('A1')->getFill()->setFillType(\PHPExcel_Style_Fill::FILL_SOLID);
$objPHPExcel->getActiveSheet(0)->getStyle('A1')->getFill()->getStartColor()->setARGB('FFCAE8EA');
```

###插入图像

```php
$objDrawing = new PHPExcel_Worksheet_Drawing();
/*设置图片路径 切记：只能是本地图片*/ 
$objDrawing->setPath('图像地址');
/*设置图片高度*/ 
$objDrawing->setHeight(180);//照片高度
$objDrawing->setWidth(150); //照片宽度
/*设置图片要插入的单元格*/
$objDrawing->setCoordinates('E2');
 /*设置图片所在单元格的格式*/
$objDrawing->setOffsetX(5);
$objDrawing->setRotation(5);
$objDrawing->getShadow()->setVisible(true);
$objDrawing->getShadow()->setDirection(50);
$objDrawing->setWorksheet($objPHPExcel->getActiveSheet());
```

###保存excel文件

```php
$objWriter = new \PHPExcel_Writer_Excel2007($objPHPExcel);
$objWriter->save("./".time().".xlsx");
```

###输出浏览器

```php
$savename = time();
// excel头参数  
header('Content-Type: application/vnd.ms-excel');  
header('Content-Disposition: attachment;filename="'.$savename.'.xls"');  //日期为文件名后缀  
header('Cache-Control: max-age=0'); 
$objWriter = \PHPExcel_IOFactory::createWriter($objPHPExcel, 'Excel5');  //excel5为xls格式，excel2007为xlsx格式  
$objWriter->save('php://output');
```

###读取excel文件

```php
//首先导入PHPExcel  
require_once 'PHPExcel.php';  
$filePath = "test.xlsx";  
//建立reader对象  
$PHPReader = new PHPExcel_Reader_Excel2007();  
if(!$PHPReader->canRead($filePath)){  
    $PHPReader = new PHPExcel_Reader_Excel5();  
    if(!$PHPReader->canRead($filePath)){  
        echo 'no Excel';  
        return ;  
    }  
}     
//建立excel对象，此时你即可以通过excel对象读取文件，也可以通过它写入文件  
$PHPExcel = $PHPReader->load($filePath);   
/**读取excel文件中的第一个工作表*/  
$currentSheet = $PHPExcel->getSheet(0);  
/**取得最大的列号*/  
$allColumn = $currentSheet->getHighestColumn();  
/**取得一共有多少行*/  
$allRow = $currentSheet->getHighestRow();    
//循环读取每个单元格的内容。注意行从1开始，列从A开始  
for($rowIndex=1;$rowIndex<=$allRow;$rowIndex++){  
    for($colIndex='A';$colIndex<=$allColumn;$colIndex++){  
        $addr = $colIndex.$rowIndex;  
        $cell = $currentSheet->getCell($addr)->getValue();  
        if($cell instanceof PHPExcel_RichText)     //富文本转换字符串  
            $cell = $cell->__toString();  
                
        echo $cell;  
        
    }  
    
}  
```