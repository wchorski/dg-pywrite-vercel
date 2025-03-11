---
{"dg-publish":true,"permalink":"/developer/css/css-animation-snips/","dgPassFrontmatter":true}
---


```html
<html lang="en"><head>
  <meta charset="UTF-8">
  

    <link rel="apple-touch-icon" type="image/png" href="https://cpwebassets.codepen.io/assets/favicon/apple-touch-icon-5ae1a0698dcc2402e9712f7d01ed509a57814f994c660df9f7a952f3060705ee.png">

    <meta name="apple-mobile-web-app-title" content="CodePen">

    <link rel="shortcut icon" type="image/x-icon" href="https://cpwebassets.codepen.io/assets/favicon/favicon-aec34940fbc1a6e787974dcd360f2c6b63348d4b1f4e06c77743096d55480f33.ico">

    <link rel="mask-icon" type="image/x-icon" href="https://cpwebassets.codepen.io/assets/favicon/logo-pin-b4b4269c16397ad2f0f7a01bcdf513a1994f4c94b8af2f191c09eb0d601762b1.svg" color="#111">



  
  <title>CodePen - Animated Hearts Step #4 - Final scene </title>
  
  
  
  
<style>
:root {
  --color-sky-top: #3e365d;
  --color-sky-bottom: #df9a91;
  --color-main: #403149;
  --color-heart: red;
  --size-viewport: 85%;
}

* {
  padding: 0;
  box-sizing: border-box;
  overflow: hidden;
}

body {
  background: var(--color-sky-bottom);
  background: linear-gradient(to bottom, var(--color-sky-top), var(--color-sky-bottom));
  width: 100vw;
  height: 100vh;
}

#hearts-alpaca {
  bottom: 33%;
  left: 40%;
}

.hearts {
  width: 5%;
  min-width: 100px;
  height: 25%;
  min-height: 300px;
  position: absolute;
  z-index: 10;
}

.heart {
  background-color: var(--color-heart);
  position: absolute;
  height: 20%;
  min-height: 15px;
  width: 20%;
  min-width: 15px;
  opacity: 0;
  -webkit-mask-image: url('data:image/svg+xml;charset=UTF-8,<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 512 512"><path fill="currentColor" d="M462.3 62.6C407.5 15.9 326 24.3 275.7 76.2L256 96.5l-19.7-20.3C186.1 24.3 104.5 15.9 49.7 62.6c-62.8 53.6-66.1 149.8-9.9 207.9l193.5 199.8c12.5 12.9 32.8 12.9 45.3 0l193.5-199.8c56.3-58.1 53-154.3-9.8-207.9z"></path></svg>');
          mask-image: url('data:image/svg+xml;charset=UTF-8,<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 512 512"><path fill="currentColor" d="M462.3 62.6C407.5 15.9 326 24.3 275.7 76.2L256 96.5l-19.7-20.3C186.1 24.3 104.5 15.9 49.7 62.6c-62.8 53.6-66.1 149.8-9.9 207.9l193.5 199.8c12.5 12.9 32.8 12.9 45.3 0l193.5-199.8c56.3-58.1 53-154.3-9.8-207.9z"></path></svg>');
  -webkit-mask-repeat: no-repeat;
          mask-repeat: no-repeat;
  -webkit-mask-position: bottom;
          mask-position: bottom;
  animation: floating-heart 10s infinite cubic-bezier(0.5, 0.5, 0.5, 0.5);
}

.heart:nth-child(1) {  
  animation-delay: 1s;
}

.heart:nth-child(2) {
  animation-delay: 2.5s;
}

.heart:nth-child(3) {
  animation-delay: 3.5s;
}

.heart:nth-child(4) {
  animation-delay: 4.5s;
}

@keyframes floating-heart {
  0% {
    opacity: 0;
    bottom: 0%;
    left: 0%;
  }
  10% {
    opacity: 1;
    bottom: 20%;
    left: 70%;
  }
  20% {
    bottom: 40%;
    left: 10%;
  }
  30% {
    bottom: 60%;
    left: 50%;
  }
  40% {
    opacity: 1;
    bottom: 80%;
    left: 5%;
  }
  48% {
    opacity: 0;
    bottom: 100%;
    left: 60%;
  }
  100% {
    opacity: 0;
    bottom: 100%;
    left: 90%;
  }
}

#alpaca {
  height: 25%;
  position: absolute;
  bottom: 8%;
  left: 40%;
  fill: var(--color-main);
}

#landscape {
  width: 100%;
  height: 10%;
  position: absolute;
  bottom: 0;
  left: 0;
  background-color: var(--color-main);
  border-top-left-radius: 50%;
  border-top-right-radius: 50%;
  z-index: 1;
}
</style>

  
  
  
</head>

<body translate="no">
  <!-- Hearts -->
  <div id="hearts-alpaca" class="hearts">
    <div class="heart"></div>
    <div class="heart"></div>
    <div class="heart"></div>
    <div class="heart"></div>
  </div>

  <!-- Alpaca -->
  <svg id="alpaca" viewBox="0 0 174 190" version="1.1" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink">
    <g>
        <path d="M158.998354,124.57 C158.548354,140.4 174.478354,150.45 171.888354,169.43 C171.748354,169.23 172.218354,170.97 171.608354,170.52 C169.268354,169.76 174.608354,174.12 173.238354,174.05 C171.898354,175.26 167.638354,172.73 171.008354,175.39 C171.938354,175.89 170.008354,175.54 169.418354,175.15 C168.628354,175.05 172.688354,178.85 172.538354,179.62 C170.908354,179.86 168.288354,177.81 168.908354,181.41 C162.998354,190.41 163.498354,190.27 152.148354,188.27 C148.578354,186.71 157.798354,185.54 155.778354,183.45 C153.198354,180.21 153.778354,163.55 151.218354,165.8 C153.658354,158.18 150.218354,164.8 151.218354,161.92 C153.218354,154.47 146.698354,173.22 149.698354,169.31 C148.448354,169.31 148.388354,175.89 147.078354,173.38 C145.218354,171.59 148.348354,174.6 145.658354,176.79 C143.908354,179.79 141.748354,179.03 138.328354,180.72 C132.328354,180.64 124.038354,181.79 130.458354,176.44 C132.558354,175.1 128.198354,174.85 129.858354,173.44 C129.858354,173.44 128.618354,173.91 128.478354,174.18 C128.058354,173.77 131.728354,169.77 130.478354,169.68 C129.938354,169.98 130.078354,170 130.288354,169.41 C134.208354,166.41 130.018354,161.17 131.178354,157.41 C132.028354,154.75 131.878354,155.73 129.778354,156.41 C127.718354,147.1 123.868354,140.41 120.328354,132 C120.258354,128.22 121.488354,128.13 118.658354,126.18 C120.208354,124.04 121.178354,124.77 118.998354,122.6 C122.348354,116.7 110.558354,128.73 107.898354,130.45 C107.898354,130.45 108.898354,129.36 108.708354,128.53 C109.018354,132.37 96.1383535,132.32 95.1183535,133.4 C95.3741592,133.048716 95.7194849,132.772455 96.1183535,132.6 C90.8183535,136.32 86.6583535,132.52 82.9283535,131.94 C85.0983535,133.53 83.2183535,137.25 85.4183535,139.18 C84.9890862,139.211063 84.5575884,139.177352 84.1383535,139.08 C84.0083535,139.38 85.5983535,140.38 84.1383535,140.08 C84.2983535,140.4 85.1383535,140.89 84.1383535,140.9 C84.9083535,141.73 82.1383535,142.9 84.4183535,143.9 C81.7583535,145.82 85.4883535,143.54 82.9683535,147.21 C83.5283535,148.59 84.5683535,154.94 83.1583535,153.61 C83.1583535,156.15 84.2283535,159.83 84.0283535,162.61 C81.1683535,157.78 84.3083535,168.5 82.8483535,170.61 C82.9883535,171.9 84.1283535,172.19 82.0683535,172.03 C81.7083535,173.85 81.0683535,176.22 80.7983535,178.56 C80.2883535,176.73 80.6683535,178.51 80.1783535,179.92 C79.1283535,181.65 78.9183535,181.24 78.1783535,183.92 C80.7683535,188.23 67.7583535,188.46 67.9683535,187.36 C69.2483535,185.75 70.7083535,181.44 68.7383535,179.8 C68.6051726,180.479695 68.5216187,181.168179 68.4883535,181.86 C68.2083535,182.33 67.7683535,178.21 67.8583535,179.31 C68.8583535,181.4 60.9883535,183.04 61.6783535,181.16 C61.6530595,179.986867 62.5149362,178.98278 63.6783535,178.83 C64.0983535,178.2 64.0783535,175.89 65.2383535,175.47 C61.6583535,176.3 65.9683535,171.67 62.3083535,175.07 C64.5283535,170.54 59.9783535,164.44 62.5983535,159.02 C63.0983535,154.62 61.3083535,153.14 60.1083535,149.5 C58.1883535,142.92 59.1083535,137.67 58.9983535,132.96 C58.2683535,133.96 57.1683535,129.27 56.3283535,130.58 C54.4583535,131.12 54.4483535,126.37 51.9483535,129.22 C35.7083535,115.68 19.6583535,79.42 21.3283535,47.6 C21.1783535,47.23 15.8783535,42.07 19.5083535,40.78 C17.5083535,37.62 14.5083535,39.25 11.3583535,39.25 C10.6283535,36.96 3.07835351,40.62 3.11835351,35.71 C-2.53164649,36.87 0.438353509,25.51 4.59835351,23.9 C4.53835351,21.9 4.23835351,22.75 2.07835351,20.65 C2.71835351,20.11 4.07835351,19.47 2.20835351,18.65 C2.48835351,18.65 4.20835351,17.9 2.63835351,17.65 C3.19835351,17.78 5.86835351,16.44 3.91835351,15.65 C5.53835351,15.9 5.21835351,14.65 8.69835351,13.39 C8.41864369,13.3719721 8.13806333,13.3719721 7.85835351,13.39 C9.85835351,11.46 17.7383535,11.28 18.2183535,8.09 C18.3383535,8.09 20.2183535,7.66 20.6983535,8.55 C19.9783535,7.8 15.2883535,0.33 19.6483535,0 C22.7783535,1.58 24.3483535,5.53 26.5683535,7.49 C27.7883535,7.58 30.7583535,7.43 30.5083535,9.08 C32.5083535,6.27 31.6383535,9.19 35.7883535,9.45 C33.0883535,13.11 38.3683535,9.45 36.4983535,13.03 C40.1383535,13.78 34.7483535,18.3 39.9083535,17.97 C38.4383535,20.1 39.6283535,20.15 40.9083535,18.85 C40.2983535,20.45 41.1883535,19.49 42.6583535,19.94 C40.5283535,20.94 40.9183535,22.24 43.0783535,23.94 C38.9983535,21.94 43.2883535,24.27 44.4383535,25.7 C41.9783535,24.76 44.8483535,27.7 45.3083535,28.92 C44.4442227,28.630878 43.6432223,28.1794668 42.9483535,27.59 C42.6883535,27.74 48.8383535,32.54 44.5683535,30.71 C45.3474883,32.3149855 46.4708008,33.7285156 47.8583535,34.85 C47.0388409,34.4564061 46.2754513,33.9553266 45.5883535,33.36 C46.1724399,34.527762 47.0385839,35.5313892 48.1083535,36.28 C47.1213945,35.7434762 46.2376511,35.0358043 45.4983535,34.19 C46.1996399,36.0268629 47.3393985,37.6644075 48.8183535,38.96 C48.1194513,38.9709431 47.4298822,38.7985508 46.8183535,38.46 C51.6783535,43.29 49.0383535,62.12 57.7183535,67.79 C92.3183535,73.21 144.528354,47.5 169.288354,92.4 C168.779597,91.4691183 168.101275,90.6415663 167.288354,89.96 C167.288354,89.96 170.498354,93.55 171.148354,101.3 C171.148354,101.3 170.148354,96.3 169.338354,95.48 C171.168354,95.91 171.518354,110.48 166.168354,118.81 C166.345712,117.819144 166.362594,116.806216 166.218354,115.81 C166.218354,115.81 165.848354,120.24 164.588354,122.53 C165.151472,120.951494 165.356421,119.267494 165.188354,117.6 C165.188354,117.6 165.468354,122.33 161.938354,124.91 C161.938354,124.91 164.678354,122.18 164.308354,119.54 C162.218354,128.72 155.748354,118.5 158.998354,124.57 Z" id="Shape"></path>
    </g>
  </svg>

  <!-- Landscape -->
  <div id="landscape"></div>
  
  
  
  
  <script src="https://cpwebassets.codepen.io/assets/editor/iframe/iframeRefreshCSS-44fe83e49b63affec96918c9af88c0d80b209a862cf87ac46bc933074b8c557d.js"></script>


</body></html>
```

```html
<div id="hearts-alpaca" class="hearts">
	<div class="heart"></div>
	<div class="heart"></div>
	<div class="heart"></div>
	<div class="heart"></div>
</div>
```

```css
:root {
  --color-sky-top: #3e365d;
  --color-sky-bottom: #df9a91;
  --color-main: #403149;
  --color-heart: red;
  --size-viewport: 85%;
}

* {
  padding: 0;
  box-sizing: border-box;
  overflow: hidden;
}

body {
  background: var(--color-sky-bottom);
  background: linear-gradient(to bottom, var(--color-sky-top), var(--color-sky-bottom));
  width: 100vw;
  height: 100vh;
}

#hearts-alpaca {
  bottom: 33%;
  left: 40%;
}

.hearts {
  width: 5%;
  min-width: 100px;
  height: 25%;
  min-height: 300px;
  position: absolute;
  z-index: 10;
}

.heart {
  background-color: var(--color-heart);
  position: absolute;
  height: 20%;
  min-height: 15px;
  width: 20%;
  min-width: 15px;
  opacity: 0;
  -webkit-mask-image: url('data:image/svg+xml;charset=UTF-8,<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 512 512"><path fill="currentColor" d="M462.3 62.6C407.5 15.9 326 24.3 275.7 76.2L256 96.5l-19.7-20.3C186.1 24.3 104.5 15.9 49.7 62.6c-62.8 53.6-66.1 149.8-9.9 207.9l193.5 199.8c12.5 12.9 32.8 12.9 45.3 0l193.5-199.8c56.3-58.1 53-154.3-9.8-207.9z"></path></svg>');
          mask-image: url('data:image/svg+xml;charset=UTF-8,<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 512 512"><path fill="currentColor" d="M462.3 62.6C407.5 15.9 326 24.3 275.7 76.2L256 96.5l-19.7-20.3C186.1 24.3 104.5 15.9 49.7 62.6c-62.8 53.6-66.1 149.8-9.9 207.9l193.5 199.8c12.5 12.9 32.8 12.9 45.3 0l193.5-199.8c56.3-58.1 53-154.3-9.8-207.9z"></path></svg>');
  -webkit-mask-repeat: no-repeat;
          mask-repeat: no-repeat;
  -webkit-mask-position: bottom;
          mask-position: bottom;
  animation: floating-heart 10s infinite cubic-bezier(0.5, 0.5, 0.5, 0.5);
}

.heart:nth-child(1) {  
  animation-delay: 1s;
}

.heart:nth-child(2) {
  animation-delay: 2.5s;
}

.heart:nth-child(3) {
  animation-delay: 3.5s;
}

.heart:nth-child(4) {
  animation-delay: 4.5s;
}

@keyframes floating-heart {
  0% {
    opacity: 0;
    bottom: 0%;
    left: 0%;
  }
  10% {
    opacity: 1;
    bottom: 20%;
    left: 70%;
  }
  20% {
    bottom: 40%;
    left: 10%;
  }
  30% {
    bottom: 60%;
    left: 50%;
  }
  40% {
    opacity: 1;
    bottom: 80%;
    left: 5%;
  }
  48% {
    opacity: 0;
    bottom: 100%;
    left: 60%;
  }
  100% {
    opacity: 0;
    bottom: 100%;
    left: 90%;
  }
}

#alpaca {
  height: 25%;
  position: absolute;
  bottom: 8%;
  left: 40%;
  fill: var(--color-main);
}

#landscape {
  width: 100%;
  height: 10%;
  position: absolute;
  bottom: 0;
  left: 0;
  background-color: var(--color-main);
  border-top-left-radius: 50%;
  border-top-right-radius: 50%;
  z-index: 1;
}
```

## Credit
- [CSS Floating hearts animation | This Dev Brain by Michal Tynior](https://thisdevbrain.com/css-floating-hearts-animation/)
- [[developer/CSS/CSS\|CSS]]