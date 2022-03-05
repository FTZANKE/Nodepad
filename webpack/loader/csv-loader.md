## Purpose

This CSV loader automatically converts data types, making it easy to import and start using data.[^这个CSV加载器自动转换数据类型，使其易于导入和开始使用数据。  ]

## Installation

Install with yarn:

```bash
yarn add -D csv-loader
```

Install with npm:

```bash
npm install --save-dev csv-loader
```

## Usage

Add the csv-loader to your webpack configuration:

```js
const config = {
    module: {
        rules: [
            {
                test: /\.csv$/,
                loader: 'csv-loader',
                options: {
                    dynamicTyping: true,
                    header: true,
                    skipEmptyLines: true
                }
            }
        ]
    }
};
```

The loader will translate csv files into JSON, with the following settings:

- automatically convert columns to the proper data type,
- parse the CSV header
- skip any blank lines in the file

## Configuration

Any options supported by Papa Parse can be passed to this loader with the options object. The current Papa Parse API is available [here](http://papaparse.com/docs#config).

## Not just a CSV loader

This module works with any column based file separated by deliminators. Simply set which extension to parse and the loader will automatically figure out which deliminator to use by default. The deliminator can also be manually set.