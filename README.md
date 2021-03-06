# Laravel Csv and Excel Validator

[![Latest Stable Version](https://poser.pugx.org/konafets/laravel-csv-validator/v/stable)](https://packagist.org/packages/konafets/laravel-csv-validator) [![Total Downloads](https://poser.pugx.org/konafets/laravel-csv-validator/downloads)](https://packagist.org/packages/konafets/laravel-csv-validator) [![Latest Unstable Version](https://poser.pugx.org/konafets/laravel-csv-validator/v/unstable)](https://packagist.org/packages/konafets/laravel-csv-validator) [![License](https://poser.pugx.org/konafets/laravel-csv-validator/license)](https://packagist.org/packages/konafets/laravel-csv-validator) [![Monthly Downloads](https://poser.pugx.org/konafets/laravel-csv-validator/d/monthly)](https://packagist.org/packages/konafets/laravel-csv-validator) 

A Laravel 5.5 package to validate csv, excel and OpenOffice data.

The idea behind the package is simple: Just use the same validation logic and syntax as in request validating.   

**The package does so far only supports CSV / Excel files with a header row**

# Installation

Execute composer command.

    composer require konafets/laravel-csv-validator:1.0-dev

Thanks to Laravels Auto-Discovery, you are done. The ServiceProvider and Alias is registered automatically.
    
# Basic usage

    $csvPath = 'employees.csv';
    $rules = [
        'First Name' => 'required|string',
        'Last Name' => 'required|string',
        'Year of Birth' => 'required|numeric',
    ];
    
    $csvValidator = CsvValidator::make($csvPath, $rules);
    
    if($csvValidator->fails()) {
        $errors = $csvValidator->getErrors();
    } else {
        $csvData = $csvValidator->getData();
    }
    
# Rules

    $rules = [
        'First Name' => 'required|string',
        'Last Name' => 'required|string',
        'Year of Birth' => 'required|numeric',
    ];

In this example, the first row of the CSV need to have three columns named `First Name`, `Last Name` and `Year of Birth`. There might be others columns as well in your file but we don't care for them.  
This keys will be used as attribute names for error message.

For more details of the rules [see](https://laravel.com/docs/5.5/validation#available-validation-rules). 

# Error messages

You can get error messages after calling fails().

    $errors = $csvValidator->getErrors();
    
    foreach ($errors as $rowIndex => $row) {
        foreach ($row as $column => $messages) {
            echo 'Row ' . $rowIndex . ', Col ' . $column . ': ' . implode(',' , $messages) . '<br>';
        }
    }

# CSV data

You also can get CSV data after calling fails().

    $csvData = $csvValidator->getData();

# Exception

The Validator throws an exception when:

* your CSV does not have any data.
* Heading key not found.

Some example code

    try {
        $csvValidator = CsvValidator::make($csv_path, $rules, $encoding);

        if($csvValidator->fails()) {
            // Do something..
        }
    } catch (\Exception $e) {
        echo $e->getMessage();
    }
    
# Credits

This package is heavily inspired by https://github.com/SUKOHI/CsvValidator. They use `maatwebsite/excel` as the underlying Excel library, while this package uses `box/sprout`.