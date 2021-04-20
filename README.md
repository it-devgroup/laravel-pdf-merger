# Laravel PDF Merger

PDF merger for Laravel inspired by another package, created for personal use. Tested with Laravel 5.6.

## Advantages
* Works with `PHP 7.4`
* Works with `Laravel 8`

## Installation
```bash
  composer require it-devgroup/laravel-pdf-merger
```

## Configuration
Make the following changes to the main configuration file located at `config/app.php`
```php
'providers' => [
   ...
   ItDevgroup\LaravelPDFMerger\Providers\PDFMergerServiceProvider::class
],

'aliases' => [
   ...
   'PDFMerger' => ItDevgroup\LaravelPDFMerger\Facades\PDFMergerFacade::class
]
```

> When merging PDFs versions above 1.4 or PDF strings, a temporary PDF will be created during the process and stored in `storage/tmp` directory, therefore you may need to create it beforehand.
> Also, note that this package requires Ghostscript installed on the server in order to functiona properly with PDF versions 1.5+. [Install Guide](https://www.ghostscript.com/doc/9.20/Install.htm)



## Usage

You can add PDFs for merging, by specifying a file path of PDF with `addPathToPDF` method, or adding PDF file as string with `addPDFString` method. The second argument of both methods is array of selected pages (`'all'` for all pages) and the third argument is PDFs orientation (portrait or landscape).
```php
$merger->addPathToPDF('/path/to/pdf', 'all', 'P');
$merger->addPDFString(file_get_contents('path/to/pdf'), ['1', '2'], 'L')
```

You can set a merged PDF name by using `setFileName` method.
```php
$merger->setFileName('merger.pdf');
```

In the end, finnish process with `merge` or `duplexMerge` method and use one of the output options for the merged PDF. The difference bwetween two methods is, that `duplexMerge` adds blank page after each merged PDF, if it has odd number of pages.

Available output options are:
  * `inline()`
  * `download()`
  * `string()`
  * `save('path/to/merged.pdf')`

```php
$merger->merge();
$merger->inline();
```

Example usage
```php
$merger = \PDFMerger::init();
$merger->addPathToPDF(base_path('/vendor/it-devgroup/laravel-pdf-merger/examples/one.pdf'), [2], 'P');
$merger->addPDFString(file_get_contents(base_path('/vendor/grofgraf/laravel-pdf-merger/examples/two.pdf')), 'all', 'L');
$merger->merge();
$merger->save(base_path('/public/pdfs/merged.pdf'));
```