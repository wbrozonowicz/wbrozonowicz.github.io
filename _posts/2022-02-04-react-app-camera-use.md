---
title: "React app - how to use webcam"
excerpt: "How to use browser (device) webcam in React app"
last_modified_at: 2022-01-21T10:27:01-05:00
categories:
  - JavaScript
tags: 
  - React
  - Camera
---

<!-- short introduction -->
## React app - how to use webcam

To use camera in web React app in class component, first install react-webcam:

{% include code-header.html %}
```js
npm install react-webcam
```
Then follow these steps: 

1. import webcam module
{% include code-header.html %}
```js
import Webcam from "react-webcam";
```

2. define state (We need 2 objects: imgSrc - it will be base64 image string, webcamRef - ref to device webcam)

{% include code-header.html %}
```js
state = {
		imgSrc:null,
		webcamRef: null,
	}
```

3. define function to set webcamRef

{% include code-header.html %}
```js
setRef = (webcam) => {
		this.webcam = webcam;
	  };
```

4. define function to capture image

{% include code-header.html %}
```js
capture = () => {
		const imageSrc = this.webcam.getScreenshot();
		this.setState({imgSrc: imageSrc})
	 };
```

5. render our Webcam component with button to capture

{% include code-header.html %}
```js
<Webcam
      audio={false}
    	ref={this.setRef}
      screenshotFormat="image/jpeg"
      />
      <button onClick={this.capture}>Capture photo</button>
      
      // below code to render captured image
        {<img
          src={this.state.imgSrc}
        />}
      
```

That's all - now You wil have base64 string in your app state, ready to further usage.

Some other options - check docs:
https://www.npmjs.com/package/react-webcam


