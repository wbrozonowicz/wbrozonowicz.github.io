---
title: "React app - read barcodes with webcam"
excerpt: "How to use browser (device) webcam in React app to scan barcodes"
last_modified_at: 2022-01-21T10:27:01-05:00
categories:
  - JavaScript
tags: 
  - React
  - Camera
  - Barcode
---

<!-- short introduction -->
## React app - how to use webcam to scan barcodes

To use camera in web React to scan barcodes follow below steps:

1] install below module:
{% include code-header.html %}
```js
npm install react-webcam-barcode-scanner
``` 

2] import webcam module
{% include code-header.html %}
```js
import BarcodeScannerComponent from "react-webcam-barcode-scanner";
```

3] we Will use this in functional component so declare data object that will get barcode text with useState:

{% include code-header.html %}
```js
const [data, setData] = React.useState("Nie ma kodu");
```

4] render our scanner component

{% include code-header.html %}
```js
{ <BarcodeScannerComponent
        width={500}
        height={500}
        onUpdate={(err, result) => {
          if (result) setData(result.text);
        //   else 
        //   setData("Not Found");
        }}
      />
      <p>{data}</p> */}
```

It work's only with https.
That's all!



