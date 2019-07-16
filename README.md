# PHP Excel

## Install

```
composer require pfinal/excel
```

##  Examples


### Import Excel

```
<?php

include 'vendor/autoload.php'; // If you don't usually need it in the framework

use PFinal\Excel\Excel;

date_default_timezone_set('PRC');

$data = Excel::readExcelFile('./1.xlsx', ['id' => 'ID', 'name' => 'NAME', 'date' => 'DATE']);

//Processing date
array_walk($data, function ($item) {
    $item['date'] = Excel::convertTime($item['date'], 'Y-m-d');
});

var_dump($data);

//If the amount of data is large, it is recommended to use the csv format.
$data = Excel::readExcelFile('./1.csv', ['id' => 'ID', 'name' => 'NAME', 'date' => 'DATE'], 1, 1, '', 'GBK');

```

Data in Excel:

![](doc/1.png)

Results:

```
$data = [
    ['id'=>1,'name'=>'Jack', 'date'=>'2017-07-18'],
    ['id'=>1,'name'=>'Mary', 'date'=>'2017-07-19'],
    ['id'=>1,'name'=>'Ethan', 'date'=>'2017-07-20'],
];
```

### Export Excel

```
$data = [
    ['id' => 1, 'name' => 'Jack', 'age' => 18, 'date'=>'2017-07-18'],
    ['id' => 2, 'name' => 'Mary', 'age' => 20, 'date'=>'2017-07-18'],
    ['id' => 3, 'name' => 'Ethan', 'age' => 34, 'date'=>'2017-07-18'],
];

$map = [
  'title'=>[
        'id' => 'ID',
        'name' => 'NAME',
        'age' => 'AGE',
    ],
];

$file = 'user' . date('Y-m-d');

//Browser download
Excel::exportExcel($data, $map, $file, 'User Info');

//Save file
//Excel::toExcelFile($data, $map, $file, 'User Info');


//Exporting to a CSV file
Excel::chunkExportCSV($map, './temp.csv', function ($writer) {

     DB::select('user')->orderBy('id')->chunk(100, function ($users) use ($writer) {
         /**  \Closure $writer */
         $writer($users);
     });
});

```
