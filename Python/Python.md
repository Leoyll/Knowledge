# Python

## module
* module = .py file
## Package
* collection of Python modules
* must include an _init_.py file
    * built-in
    * third-party
        * need to intall
        * PyPI: Python Package Index
        * pip: a package manager for Python
            * pip install XXX
## Library
* collection of packages

## IO operations
### built-in package
* io module
    * create, read, write
### third-party package
* pip install openpyxl
* openpyxl: read/write Excel xlsx/xlsm/xltx/xltm files
    * excel_file = openpyxl.load_workbook("excelName.xls")
    * spreadsheetContent = excel_file["sheetName"]
    * spreadsheetContent.max_row: row numbers
```python=
for rowIndex in range(2, spreadsheetContent.max_row + 1):
    cellValue = spreadsheetContent.cell(rowIndex, 4).value
    
for rowArrContent in spreadsheetContent:  #loop for the rows of a spreadsheet
    for cell in rowArrContent:            #loop for the cells of one row
        print(cell.value, end='   ')      #end='' means no line feed
```



## Grammer
### 缩进
* 缩进对于python来说是语法的一部分
* space几乎所有的编辑器、所有的OS环境都是一样的，而对于tab的处理却不尽相同
* 所以，python使用space缩进

### loop: for
```python=
range(3)        #[0,1,2]
range(1,5)      #[1,2,3,4]
```

### map
```python=
exampleMap = {}
exampleMap = {"key1":2, "key2":25}
if keyName1 in exampleMap:
    value = exampleMap[keyName1]
    #do something
else:
    exampleMap[keyName1] = valueOfKeyName1    #if no key, add it
```





<details>
<summary>点击显示答案</summary>
自己
</details>
