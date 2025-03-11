---
{"dg-publish":true,"permalink":"/developer/css/css-animated-bubble-background/","dgPassFrontmatter":true}
---

```html
<div class='canvas'>
  <div class='bubble'></div>
  <div class='bubble'></div>
  <div class='bubble'></div>
  <div class='bubble'></div>
  <div class='bubble'></div>
  <div class='bubble'></div>
  <div class='bubble'></div>
  <div class='bubble'></div>
  <div class='bubble'></div>
  <div class='bubble'></div>
  <div class='bubble'></div>
  <div class='bubble'></div>
  <div class='bubble'></div>
  <div class='bubble'></div>
  <div class='bubble'></div>
  <div class='bubble'></div>
  <div class='bubble'></div>
  <div class='bubble'></div>
  <div class='bubble'></div>
  <div class='bubble'></div>
  <div class='bubble'></div>
  <div class='bubble'></div>
  <div class='bubble'></div>
  <div class='bubble'></div>
  <div class='bubble'></div>
  <div class='bubble'></div>
  <div class='bubble'></div>
  <div class='bubble'></div>
  <div class='bubble'></div>
  <div class='bubble'></div>
  <div class='bubble'></div>
  <div class='bubble'></div>
  <div class='bubble'></div>
  <div class='bubble'></div>
  <div class='bubble'></div>
  <div class='bubble'></div>
  <div class='bubble'></div>
  <div class='bubble'></div>
  <div class='bubble'></div>
  <div class='bubble'></div>
  <div class='bubble'></div>
  <div class='bubble'></div>
  <div class='bubble'></div>
  <div class='bubble'></div>
  <div class='bubble'></div>
  <div class='bubble'></div>
  <div class='bubble'></div>
  <div class='bubble'></div>
  <div class='bubble'></div>
  <div class='bubble'></div>
</div>

```

```css
.canvas {
  height: 100vh;
  width: 100vw;
  background: linear-gradient(to bottom, #edfffa 0%, #31c5d6 100%);
  position: relative;
  overflow: hidden;
}
.bubble {
  display: block;
  border-radius: 100%;
  opacity: 0.8;
  position: absolute;
}
.bubble:nth-child(1) {
  background: radial-gradient(ellipse at center, #b8c6c6 0%, #30b3d3 46%, #20628c 100%);
  width: 97px;
  height: 97px;
  left: 21vw;
  bottom: 6vh;
  -webkit-animation: move1 infinite 13s;
          animation: move1 infinite 13s;
}
@-webkit-keyframes move1 {
  0% {
    bottom: -100px;
  }
  100% {
    bottom: 11vh;
    transform: translate(2px, 0);
    opacity: 0;
  }
}
@keyframes move1 {
  0% {
    bottom: -100px;
  }
  100% {
    bottom: 11vh;
    transform: translate(2px, 0);
    opacity: 0;
  }
}
.bubble:nth-child(2) {
  background: radial-gradient(ellipse at top right, #b8c6c6 0%, #30b3d3 46%, #20628c 100%);
  width: 100px;
  height: 100px;
  left: 75vw;
  bottom: 98vh;
  -webkit-animation: move2 infinite 14s;
          animation: move2 infinite 14s;
}
@-webkit-keyframes move2 {
  0% {
    bottom: -100px;
  }
  100% {
    bottom: 59vh;
    transform: translate(-49px, 0);
    opacity: 0;
  }
}
@keyframes move2 {
  0% {
    bottom: -100px;
  }
  100% {
    bottom: 59vh;
    transform: translate(-49px, 0);
    opacity: 0;
  }
}
.bubble:nth-child(3) {
  background: radial-gradient(ellipse at center, #b8c6c6 0%, #30b3d3 46%, #20628c 100%);
  width: 79px;
  height: 79px;
  left: 33vw;
  bottom: 84vh;
  -webkit-animation: move3 infinite 5s;
          animation: move3 infinite 5s;
}
@-webkit-keyframes move3 {
  0% {
    bottom: -100px;
  }
  100% {
    bottom: 59vh;
    transform: translate(116px, 0);
    opacity: 0;
  }
}
@keyframes move3 {
  0% {
    bottom: -100px;
  }
  100% {
    bottom: 59vh;
    transform: translate(116px, 0);
    opacity: 0;
  }
}
.bubble:nth-child(4) {
  background: radial-gradient(ellipse at top right, #b8c6c6 0%, #30b3d3 46%, #20628c 100%);
  width: 84px;
  height: 84px;
  left: 39vw;
  bottom: 23vh;
  -webkit-animation: move4 infinite 8s;
          animation: move4 infinite 8s;
}
@-webkit-keyframes move4 {
  0% {
    bottom: -100px;
  }
  100% {
    bottom: 53vh;
    transform: translate(106px, 0);
    opacity: 0;
  }
}
@keyframes move4 {
  0% {
    bottom: -100px;
  }
  100% {
    bottom: 53vh;
    transform: translate(106px, 0);
    opacity: 0;
  }
}
.bubble:nth-child(5) {
  background: radial-gradient(ellipse at center, #b8c6c6 0%, #30b3d3 46%, #20628c 100%);
  width: 42px;
  height: 42px;
  left: 45vw;
  bottom: 82vh;
  -webkit-animation: move5 infinite 5s;
          animation: move5 infinite 5s;
}
@-webkit-keyframes move5 {
  0% {
    bottom: -100px;
  }
  100% {
    bottom: 26vh;
    transform: translate(60px, 0);
    opacity: 0;
  }
}
@keyframes move5 {
  0% {
    bottom: -100px;
  }
  100% {
    bottom: 26vh;
    transform: translate(60px, 0);
    opacity: 0;
  }
}
.bubble:nth-child(6) {
  background: radial-gradient(ellipse at top right, #b8c6c6 0%, #30b3d3 46%, #20628c 100%);
  width: 7px;
  height: 7px;
  left: 8vw;
  bottom: 27vh;
  -webkit-animation: move6 infinite 11s;
          animation: move6 infinite 11s;
}
@-webkit-keyframes move6 {
  0% {
    bottom: -100px;
  }
  100% {
    bottom: 66vh;
    transform: translate(143px, 0);
    opacity: 0;
  }
}
@keyframes move6 {
  0% {
    bottom: -100px;
  }
  100% {
    bottom: 66vh;
    transform: translate(143px, 0);
    opacity: 0;
  }
}
.bubble:nth-child(7) {
  background: radial-gradient(ellipse at center, #b8c6c6 0%, #30b3d3 46%, #20628c 100%);
  width: 86px;
  height: 86px;
  left: 43vw;
  bottom: 62vh;
  -webkit-animation: move7 infinite 9s;
          animation: move7 infinite 9s;
}
@-webkit-keyframes move7 {
  0% {
    bottom: -100px;
  }
  100% {
    bottom: 45vh;
    transform: translate(86px, 0);
    opacity: 0;
  }
}
@keyframes move7 {
  0% {
    bottom: -100px;
  }
  100% {
    bottom: 45vh;
    transform: translate(86px, 0);
    opacity: 0;
  }
}
.bubble:nth-child(8) {
  background: radial-gradient(ellipse at top right, #b8c6c6 0%, #30b3d3 46%, #20628c 100%);
  width: 86px;
  height: 86px;
  left: 93vw;
  bottom: 66vh;
  -webkit-animation: move8 infinite 7s;
          animation: move8 infinite 7s;
}
@-webkit-keyframes move8 {
  0% {
    bottom: -100px;
  }
  100% {
    bottom: 75vh;
    transform: translate(-16px, 0);
    opacity: 0;
  }
}
@keyframes move8 {
  0% {
    bottom: -100px;
  }
  100% {
    bottom: 75vh;
    transform: translate(-16px, 0);
    opacity: 0;
  }
}
.bubble:nth-child(9) {
  background: radial-gradient(ellipse at center, #b8c6c6 0%, #30b3d3 46%, #20628c 100%);
  width: 48px;
  height: 48px;
  left: 47vw;
  bottom: 93vh;
  -webkit-animation: move9 infinite 4s;
          animation: move9 infinite 4s;
}
@-webkit-keyframes move9 {
  0% {
    bottom: -100px;
  }
  100% {
    bottom: 74vh;
    transform: translate(30px, 0);
    opacity: 0;
  }
}
@keyframes move9 {
  0% {
    bottom: -100px;
  }
  100% {
    bottom: 74vh;
    transform: translate(30px, 0);
    opacity: 0;
  }
}
.bubble:nth-child(10) {
  background: radial-gradient(ellipse at top right, #b8c6c6 0%, #30b3d3 46%, #20628c 100%);
  width: 72px;
  height: 72px;
  left: 75vw;
  bottom: 2vh;
  -webkit-animation: move10 infinite 9s;
          animation: move10 infinite 9s;
}
@-webkit-keyframes move10 {
  0% {
    bottom: -100px;
  }
  100% {
    bottom: 55vh;
    transform: translate(189px, 0);
    opacity: 0;
  }
}
@keyframes move10 {
  0% {
    bottom: -100px;
  }
  100% {
    bottom: 55vh;
    transform: translate(189px, 0);
    opacity: 0;
  }
}
.bubble:nth-child(11) {
  background: radial-gradient(ellipse at center, #b8c6c6 0%, #30b3d3 46%, #20628c 100%);
  width: 46px;
  height: 46px;
  left: 78vw;
  bottom: 62vh;
  -webkit-animation: move11 infinite 5s;
          animation: move11 infinite 5s;
}
@-webkit-keyframes move11 {
  0% {
    bottom: -100px;
  }
  100% {
    bottom: 73vh;
    transform: translate(-42px, 0);
    opacity: 0;
  }
}
@keyframes move11 {
  0% {
    bottom: -100px;
  }
  100% {
    bottom: 73vh;
    transform: translate(-42px, 0);
    opacity: 0;
  }
}
.bubble:nth-child(12) {
  background: radial-gradient(ellipse at top right, #b8c6c6 0%, #30b3d3 46%, #20628c 100%);
  width: 38px;
  height: 38px;
  left: 39vw;
  bottom: 22vh;
  -webkit-animation: move12 infinite 11s;
          animation: move12 infinite 11s;
}
@-webkit-keyframes move12 {
  0% {
    bottom: -100px;
  }
  100% {
    bottom: 5vh;
    transform: translate(-97px, 0);
    opacity: 0;
  }
}
@keyframes move12 {
  0% {
    bottom: -100px;
  }
  100% {
    bottom: 5vh;
    transform: translate(-97px, 0);
    opacity: 0;
  }
}
.bubble:nth-child(13) {
  background: radial-gradient(ellipse at center, #b8c6c6 0%, #30b3d3 46%, #20628c 100%);
  width: 32px;
  height: 32px;
  left: 65vw;
  bottom: 19vh;
  -webkit-animation: move13 infinite 13s;
          animation: move13 infinite 13s;
}
@-webkit-keyframes move13 {
  0% {
    bottom: -100px;
  }
  100% {
    bottom: 1vh;
    transform: translate(16px, 0);
    opacity: 0;
  }
}
@keyframes move13 {
  0% {
    bottom: -100px;
  }
  100% {
    bottom: 1vh;
    transform: translate(16px, 0);
    opacity: 0;
  }
}
.bubble:nth-child(14) {
  background: radial-gradient(ellipse at top right, #b8c6c6 0%, #30b3d3 46%, #20628c 100%);
  width: 6px;
  height: 6px;
  left: 40vw;
  bottom: 42vh;
  -webkit-animation: move14 infinite 11s;
          animation: move14 infinite 11s;
}
@-webkit-keyframes move14 {
  0% {
    bottom: -100px;
  }
  100% {
    bottom: 74vh;
    transform: translate(-96px, 0);
    opacity: 0;
  }
}
@keyframes move14 {
  0% {
    bottom: -100px;
  }
  100% {
    bottom: 74vh;
    transform: translate(-96px, 0);
    opacity: 0;
  }
}
.bubble:nth-child(15) {
  background: radial-gradient(ellipse at center, #b8c6c6 0%, #30b3d3 46%, #20628c 100%);
  width: 96px;
  height: 96px;
  left: 80vw;
  bottom: 69vh;
  -webkit-animation: move15 infinite 12s;
          animation: move15 infinite 12s;
}
@-webkit-keyframes move15 {
  0% {
    bottom: -100px;
  }
  100% {
    bottom: 76vh;
    transform: translate(-66px, 0);
    opacity: 0;
  }
}
@keyframes move15 {
  0% {
    bottom: -100px;
  }
  100% {
    bottom: 76vh;
    transform: translate(-66px, 0);
    opacity: 0;
  }
}
.bubble:nth-child(16) {
  background: radial-gradient(ellipse at top right, #b8c6c6 0%, #30b3d3 46%, #20628c 100%);
  width: 86px;
  height: 86px;
  left: 22vw;
  bottom: 80vh;
  -webkit-animation: move16 infinite 3s;
          animation: move16 infinite 3s;
}
@-webkit-keyframes move16 {
  0% {
    bottom: -100px;
  }
  100% {
    bottom: 77vh;
    transform: translate(7px, 0);
    opacity: 0;
  }
}
@keyframes move16 {
  0% {
    bottom: -100px;
  }
  100% {
    bottom: 77vh;
    transform: translate(7px, 0);
    opacity: 0;
  }
}
.bubble:nth-child(17) {
  background: radial-gradient(ellipse at center, #b8c6c6 0%, #30b3d3 46%, #20628c 100%);
  width: 72px;
  height: 72px;
  left: 40vw;
  bottom: 38vh;
  -webkit-animation: move17 infinite 14s;
          animation: move17 infinite 14s;
}
@-webkit-keyframes move17 {
  0% {
    bottom: -100px;
  }
  100% {
    bottom: 63vh;
    transform: translate(199px, 0);
    opacity: 0;
  }
}
@keyframes move17 {
  0% {
    bottom: -100px;
  }
  100% {
    bottom: 63vh;
    transform: translate(199px, 0);
    opacity: 0;
  }
}
.bubble:nth-child(18) {
  background: radial-gradient(ellipse at top right, #b8c6c6 0%, #30b3d3 46%, #20628c 100%);
  width: 65px;
  height: 65px;
  left: 99vw;
  bottom: 40vh;
  -webkit-animation: move18 infinite 15s;
          animation: move18 infinite 15s;
}
@-webkit-keyframes move18 {
  0% {
    bottom: -100px;
  }
  100% {
    bottom: 63vh;
    transform: translate(-4px, 0);
    opacity: 0;
  }
}
@keyframes move18 {
  0% {
    bottom: -100px;
  }
  100% {
    bottom: 63vh;
    transform: translate(-4px, 0);
    opacity: 0;
  }
}
.bubble:nth-child(19) {
  background: radial-gradient(ellipse at center, #b8c6c6 0%, #30b3d3 46%, #20628c 100%);
  width: 31px;
  height: 31px;
  left: 82vw;
  bottom: 74vh;
  -webkit-animation: move19 infinite 6s;
          animation: move19 infinite 6s;
}
@-webkit-keyframes move19 {
  0% {
    bottom: -100px;
  }
  100% {
    bottom: 9vh;
    transform: translate(-97px, 0);
    opacity: 0;
  }
}
@keyframes move19 {
  0% {
    bottom: -100px;
  }
  100% {
    bottom: 9vh;
    transform: translate(-97px, 0);
    opacity: 0;
  }
}
.bubble:nth-child(20) {
  background: radial-gradient(ellipse at top right, #b8c6c6 0%, #30b3d3 46%, #20628c 100%);
  width: 85px;
  height: 85px;
  left: 83vw;
  bottom: 33vh;
  -webkit-animation: move20 infinite 4s;
          animation: move20 infinite 4s;
}
@-webkit-keyframes move20 {
  0% {
    bottom: -100px;
  }
  100% {
    bottom: 72vh;
    transform: translate(16px, 0);
    opacity: 0;
  }
}
@keyframes move20 {
  0% {
    bottom: -100px;
  }
  100% {
    bottom: 72vh;
    transform: translate(16px, 0);
    opacity: 0;
  }
}
.bubble:nth-child(21) {
  background: radial-gradient(ellipse at center, #b8c6c6 0%, #30b3d3 46%, #20628c 100%);
  width: 45px;
  height: 45px;
  left: 15vw;
  bottom: 71vh;
  -webkit-animation: move21 infinite 5s;
          animation: move21 infinite 5s;
}
@-webkit-keyframes move21 {
  0% {
    bottom: -100px;
  }
  100% {
    bottom: 51vh;
    transform: translate(104px, 0);
    opacity: 0;
  }
}
@keyframes move21 {
  0% {
    bottom: -100px;
  }
  100% {
    bottom: 51vh;
    transform: translate(104px, 0);
    opacity: 0;
  }
}
.bubble:nth-child(22) {
  background: radial-gradient(ellipse at top right, #b8c6c6 0%, #30b3d3 46%, #20628c 100%);
  width: 6px;
  height: 6px;
  left: 14vw;
  bottom: 58vh;
  -webkit-animation: move22 infinite 9s;
          animation: move22 infinite 9s;
}
@-webkit-keyframes move22 {
  0% {
    bottom: -100px;
  }
  100% {
    bottom: 26vh;
    transform: translate(-91px, 0);
    opacity: 0;
  }
}
@keyframes move22 {
  0% {
    bottom: -100px;
  }
  100% {
    bottom: 26vh;
    transform: translate(-91px, 0);
    opacity: 0;
  }
}
.bubble:nth-child(23) {
  background: radial-gradient(ellipse at center, #b8c6c6 0%, #30b3d3 46%, #20628c 100%);
  width: 83px;
  height: 83px;
  left: 56vw;
  bottom: 72vh;
  -webkit-animation: move23 infinite 13s;
          animation: move23 infinite 13s;
}
@-webkit-keyframes move23 {
  0% {
    bottom: -100px;
  }
  100% {
    bottom: 45vh;
    transform: translate(77px, 0);
    opacity: 0;
  }
}
@keyframes move23 {
  0% {
    bottom: -100px;
  }
  100% {
    bottom: 45vh;
    transform: translate(77px, 0);
    opacity: 0;
  }
}
.bubble:nth-child(24) {
  background: radial-gradient(ellipse at top right, #b8c6c6 0%, #30b3d3 46%, #20628c 100%);
  width: 54px;
  height: 54px;
  left: 16vw;
  bottom: 85vh;
  -webkit-animation: move24 infinite 9s;
          animation: move24 infinite 9s;
}
@-webkit-keyframes move24 {
  0% {
    bottom: -100px;
  }
  100% {
    bottom: 76vh;
    transform: translate(137px, 0);
    opacity: 0;
  }
}
@keyframes move24 {
  0% {
    bottom: -100px;
  }
  100% {
    bottom: 76vh;
    transform: translate(137px, 0);
    opacity: 0;
  }
}
.bubble:nth-child(25) {
  background: radial-gradient(ellipse at center, #b8c6c6 0%, #30b3d3 46%, #20628c 100%);
  width: 100px;
  height: 100px;
  left: 10vw;
  bottom: 81vh;
  -webkit-animation: move25 infinite 3s;
          animation: move25 infinite 3s;
}
@-webkit-keyframes move25 {
  0% {
    bottom: -100px;
  }
  100% {
    bottom: 40vh;
    transform: translate(-1px, 0);
    opacity: 0;
  }
}
@keyframes move25 {
  0% {
    bottom: -100px;
  }
  100% {
    bottom: 40vh;
    transform: translate(-1px, 0);
    opacity: 0;
  }
}
.bubble:nth-child(26) {
  background: radial-gradient(ellipse at top right, #b8c6c6 0%, #30b3d3 46%, #20628c 100%);
  width: 24px;
  height: 24px;
  left: 6vw;
  bottom: 45vh;
  -webkit-animation: move26 infinite 8s;
          animation: move26 infinite 8s;
}
@-webkit-keyframes move26 {
  0% {
    bottom: -100px;
  }
  100% {
    bottom: 61vh;
    transform: translate(88px, 0);
    opacity: 0;
  }
}
@keyframes move26 {
  0% {
    bottom: -100px;
  }
  100% {
    bottom: 61vh;
    transform: translate(88px, 0);
    opacity: 0;
  }
}
.bubble:nth-child(27) {
  background: radial-gradient(ellipse at center, #b8c6c6 0%, #30b3d3 46%, #20628c 100%);
  width: 11px;
  height: 11px;
  left: 22vw;
  bottom: 78vh;
  -webkit-animation: move27 infinite 9s;
          animation: move27 infinite 9s;
}
@-webkit-keyframes move27 {
  0% {
    bottom: -100px;
  }
  100% {
    bottom: 52vh;
    transform: translate(-95px, 0);
    opacity: 0;
  }
}
@keyframes move27 {
  0% {
    bottom: -100px;
  }
  100% {
    bottom: 52vh;
    transform: translate(-95px, 0);
    opacity: 0;
  }
}
.bubble:nth-child(28) {
  background: radial-gradient(ellipse at top right, #b8c6c6 0%, #30b3d3 46%, #20628c 100%);
  width: 24px;
  height: 24px;
  left: 92vw;
  bottom: 95vh;
  -webkit-animation: move28 infinite 9s;
          animation: move28 infinite 9s;
}
@-webkit-keyframes move28 {
  0% {
    bottom: -100px;
  }
  100% {
    bottom: 65vh;
    transform: translate(-47px, 0);
    opacity: 0;
  }
}
@keyframes move28 {
  0% {
    bottom: -100px;
  }
  100% {
    bottom: 65vh;
    transform: translate(-47px, 0);
    opacity: 0;
  }
}
.bubble:nth-child(29) {
  background: radial-gradient(ellipse at center, #b8c6c6 0%, #30b3d3 46%, #20628c 100%);
  width: 64px;
  height: 64px;
  left: 77vw;
  bottom: 78vh;
  -webkit-animation: move29 infinite 4s;
          animation: move29 infinite 4s;
}
@-webkit-keyframes move29 {
  0% {
    bottom: -100px;
  }
  100% {
    bottom: 75vh;
    transform: translate(151px, 0);
    opacity: 0;
  }
}
@keyframes move29 {
  0% {
    bottom: -100px;
  }
  100% {
    bottom: 75vh;
    transform: translate(151px, 0);
    opacity: 0;
  }
}
.bubble:nth-child(30) {
  background: radial-gradient(ellipse at top right, #b8c6c6 0%, #30b3d3 46%, #20628c 100%);
  width: 98px;
  height: 98px;
  left: 97vw;
  bottom: 60vh;
  -webkit-animation: move30 infinite 4s;
          animation: move30 infinite 4s;
}
@-webkit-keyframes move30 {
  0% {
    bottom: -100px;
  }
  100% {
    bottom: 35vh;
    transform: translate(-52px, 0);
    opacity: 0;
  }
}
@keyframes move30 {
  0% {
    bottom: -100px;
  }
  100% {
    bottom: 35vh;
    transform: translate(-52px, 0);
    opacity: 0;
  }
}
.bubble:nth-child(31) {
  background: radial-gradient(ellipse at center, #b8c6c6 0%, #30b3d3 46%, #20628c 100%);
  width: 45px;
  height: 45px;
  left: 25vw;
  bottom: 41vh;
  -webkit-animation: move31 infinite 11s;
          animation: move31 infinite 11s;
}
@-webkit-keyframes move31 {
  0% {
    bottom: -100px;
  }
  100% {
    bottom: 88vh;
    transform: translate(-62px, 0);
    opacity: 0;
  }
}
@keyframes move31 {
  0% {
    bottom: -100px;
  }
  100% {
    bottom: 88vh;
    transform: translate(-62px, 0);
    opacity: 0;
  }
}
.bubble:nth-child(32) {
  background: radial-gradient(ellipse at top right, #b8c6c6 0%, #30b3d3 46%, #20628c 100%);
  width: 64px;
  height: 64px;
  left: 39vw;
  bottom: 32vh;
  -webkit-animation: move32 infinite 15s;
          animation: move32 infinite 15s;
}
@-webkit-keyframes move32 {
  0% {
    bottom: -100px;
  }
  100% {
    bottom: 48vh;
    transform: translate(116px, 0);
    opacity: 0;
  }
}
@keyframes move32 {
  0% {
    bottom: -100px;
  }
  100% {
    bottom: 48vh;
    transform: translate(116px, 0);
    opacity: 0;
  }
}
.bubble:nth-child(33) {
  background: radial-gradient(ellipse at center, #b8c6c6 0%, #30b3d3 46%, #20628c 100%);
  width: 44px;
  height: 44px;
  left: 71vw;
  bottom: 36vh;
  -webkit-animation: move33 infinite 8s;
          animation: move33 infinite 8s;
}
@-webkit-keyframes move33 {
  0% {
    bottom: -100px;
  }
  100% {
    bottom: 100vh;
    transform: translate(26px, 0);
    opacity: 0;
  }
}
@keyframes move33 {
  0% {
    bottom: -100px;
  }
  100% {
    bottom: 100vh;
    transform: translate(26px, 0);
    opacity: 0;
  }
}
.bubble:nth-child(34) {
  background: radial-gradient(ellipse at top right, #b8c6c6 0%, #30b3d3 46%, #20628c 100%);
  width: 78px;
  height: 78px;
  left: 95vw;
  bottom: 25vh;
  -webkit-animation: move34 infinite 5s;
          animation: move34 infinite 5s;
}
@-webkit-keyframes move34 {
  0% {
    bottom: -100px;
  }
  100% {
    bottom: 63vh;
    transform: translate(192px, 0);
    opacity: 0;
  }
}
@keyframes move34 {
  0% {
    bottom: -100px;
  }
  100% {
    bottom: 63vh;
    transform: translate(192px, 0);
    opacity: 0;
  }
}
.bubble:nth-child(35) {
  background: radial-gradient(ellipse at center, #b8c6c6 0%, #30b3d3 46%, #20628c 100%);
  width: 8px;
  height: 8px;
  left: 59vw;
  bottom: 45vh;
  -webkit-animation: move35 infinite 15s;
          animation: move35 infinite 15s;
}
@-webkit-keyframes move35 {
  0% {
    bottom: -100px;
  }
  100% {
    bottom: 3vh;
    transform: translate(172px, 0);
    opacity: 0;
  }
}
@keyframes move35 {
  0% {
    bottom: -100px;
  }
  100% {
    bottom: 3vh;
    transform: translate(172px, 0);
    opacity: 0;
  }
}
.bubble:nth-child(36) {
  background: radial-gradient(ellipse at top right, #b8c6c6 0%, #30b3d3 46%, #20628c 100%);
  width: 80px;
  height: 80px;
  left: 96vw;
  bottom: 95vh;
  -webkit-animation: move36 infinite 5s;
          animation: move36 infinite 5s;
}
@-webkit-keyframes move36 {
  0% {
    bottom: -100px;
  }
  100% {
    bottom: 41vh;
    transform: translate(176px, 0);
    opacity: 0;
  }
}
@keyframes move36 {
  0% {
    bottom: -100px;
  }
  100% {
    bottom: 41vh;
    transform: translate(176px, 0);
    opacity: 0;
  }
}
.bubble:nth-child(37) {
  background: radial-gradient(ellipse at center, #b8c6c6 0%, #30b3d3 46%, #20628c 100%);
  width: 64px;
  height: 64px;
  left: 12vw;
  bottom: 9vh;
  -webkit-animation: move37 infinite 8s;
          animation: move37 infinite 8s;
}
@-webkit-keyframes move37 {
  0% {
    bottom: -100px;
  }
  100% {
    bottom: 9vh;
    transform: translate(101px, 0);
    opacity: 0;
  }
}
@keyframes move37 {
  0% {
    bottom: -100px;
  }
  100% {
    bottom: 9vh;
    transform: translate(101px, 0);
    opacity: 0;
  }
}
.bubble:nth-child(38) {
  background: radial-gradient(ellipse at top right, #b8c6c6 0%, #30b3d3 46%, #20628c 100%);
  width: 31px;
  height: 31px;
  left: 72vw;
  bottom: 62vh;
  -webkit-animation: move38 infinite 10s;
          animation: move38 infinite 10s;
}
@-webkit-keyframes move38 {
  0% {
    bottom: -100px;
  }
  100% {
    bottom: 14vh;
    transform: translate(4px, 0);
    opacity: 0;
  }
}
@keyframes move38 {
  0% {
    bottom: -100px;
  }
  100% {
    bottom: 14vh;
    transform: translate(4px, 0);
    opacity: 0;
  }
}
.bubble:nth-child(39) {
  background: radial-gradient(ellipse at center, #b8c6c6 0%, #30b3d3 46%, #20628c 100%);
  width: 70px;
  height: 70px;
  left: 91vw;
  bottom: 3vh;
  -webkit-animation: move39 infinite 13s;
          animation: move39 infinite 13s;
}
@-webkit-keyframes move39 {
  0% {
    bottom: -100px;
  }
  100% {
    bottom: 48vh;
    transform: translate(69px, 0);
    opacity: 0;
  }
}
@keyframes move39 {
  0% {
    bottom: -100px;
  }
  100% {
    bottom: 48vh;
    transform: translate(69px, 0);
    opacity: 0;
  }
}
.bubble:nth-child(40) {
  background: radial-gradient(ellipse at top right, #b8c6c6 0%, #30b3d3 46%, #20628c 100%);
  width: 13px;
  height: 13px;
  left: 21vw;
  bottom: 22vh;
  -webkit-animation: move40 infinite 5s;
          animation: move40 infinite 5s;
}
@-webkit-keyframes move40 {
  0% {
    bottom: -100px;
  }
  100% {
    bottom: 59vh;
    transform: translate(1px, 0);
    opacity: 0;
  }
}
@keyframes move40 {
  0% {
    bottom: -100px;
  }
  100% {
    bottom: 59vh;
    transform: translate(1px, 0);
    opacity: 0;
  }
}
.bubble:nth-child(41) {
  background: radial-gradient(ellipse at center, #b8c6c6 0%, #30b3d3 46%, #20628c 100%);
  width: 76px;
  height: 76px;
  left: 37vw;
  bottom: 1vh;
  -webkit-animation: move41 infinite 15s;
          animation: move41 infinite 15s;
}
@-webkit-keyframes move41 {
  0% {
    bottom: -100px;
  }
  100% {
    bottom: 68vh;
    transform: translate(2px, 0);
    opacity: 0;
  }
}
@keyframes move41 {
  0% {
    bottom: -100px;
  }
  100% {
    bottom: 68vh;
    transform: translate(2px, 0);
    opacity: 0;
  }
}
.bubble:nth-child(42) {
  background: radial-gradient(ellipse at top right, #b8c6c6 0%, #30b3d3 46%, #20628c 100%);
  width: 46px;
  height: 46px;
  left: 99vw;
  bottom: 27vh;
  -webkit-animation: move42 infinite 4s;
          animation: move42 infinite 4s;
}
@-webkit-keyframes move42 {
  0% {
    bottom: -100px;
  }
  100% {
    bottom: 10vh;
    transform: translate(74px, 0);
    opacity: 0;
  }
}
@keyframes move42 {
  0% {
    bottom: -100px;
  }
  100% {
    bottom: 10vh;
    transform: translate(74px, 0);
    opacity: 0;
  }
}
.bubble:nth-child(43) {
  background: radial-gradient(ellipse at center, #b8c6c6 0%, #30b3d3 46%, #20628c 100%);
  width: 34px;
  height: 34px;
  left: 44vw;
  bottom: 11vh;
  -webkit-animation: move43 infinite 3s;
          animation: move43 infinite 3s;
}
@-webkit-keyframes move43 {
  0% {
    bottom: -100px;
  }
  100% {
    bottom: 40vh;
    transform: translate(158px, 0);
    opacity: 0;
  }
}
@keyframes move43 {
  0% {
    bottom: -100px;
  }
  100% {
    bottom: 40vh;
    transform: translate(158px, 0);
    opacity: 0;
  }
}
.bubble:nth-child(44) {
  background: radial-gradient(ellipse at top right, #b8c6c6 0%, #30b3d3 46%, #20628c 100%);
  width: 87px;
  height: 87px;
  left: 37vw;
  bottom: 63vh;
  -webkit-animation: move44 infinite 5s;
          animation: move44 infinite 5s;
}
@-webkit-keyframes move44 {
  0% {
    bottom: -100px;
  }
  100% {
    bottom: 79vh;
    transform: translate(123px, 0);
    opacity: 0;
  }
}
@keyframes move44 {
  0% {
    bottom: -100px;
  }
  100% {
    bottom: 79vh;
    transform: translate(123px, 0);
    opacity: 0;
  }
}
.bubble:nth-child(45) {
  background: radial-gradient(ellipse at center, #b8c6c6 0%, #30b3d3 46%, #20628c 100%);
  width: 6px;
  height: 6px;
  left: 9vw;
  bottom: 2vh;
  -webkit-animation: move45 infinite 4s;
          animation: move45 infinite 4s;
}
@-webkit-keyframes move45 {
  0% {
    bottom: -100px;
  }
  100% {
    bottom: 90vh;
    transform: translate(149px, 0);
    opacity: 0;
  }
}
@keyframes move45 {
  0% {
    bottom: -100px;
  }
  100% {
    bottom: 90vh;
    transform: translate(149px, 0);
    opacity: 0;
  }
}
.bubble:nth-child(46) {
  background: radial-gradient(ellipse at top right, #b8c6c6 0%, #30b3d3 46%, #20628c 100%);
  width: 78px;
  height: 78px;
  left: 15vw;
  bottom: 31vh;
  -webkit-animation: move46 infinite 3s;
          animation: move46 infinite 3s;
}
@-webkit-keyframes move46 {
  0% {
    bottom: -100px;
  }
  100% {
    bottom: 82vh;
    transform: translate(158px, 0);
    opacity: 0;
  }
}
@keyframes move46 {
  0% {
    bottom: -100px;
  }
  100% {
    bottom: 82vh;
    transform: translate(158px, 0);
    opacity: 0;
  }
}
.bubble:nth-child(47) {
  background: radial-gradient(ellipse at center, #b8c6c6 0%, #30b3d3 46%, #20628c 100%);
  width: 60px;
  height: 60px;
  left: 80vw;
  bottom: 88vh;
  -webkit-animation: move47 infinite 14s;
          animation: move47 infinite 14s;
}
@-webkit-keyframes move47 {
  0% {
    bottom: -100px;
  }
  100% {
    bottom: 27vh;
    transform: translate(136px, 0);
    opacity: 0;
  }
}
@keyframes move47 {
  0% {
    bottom: -100px;
  }
  100% {
    bottom: 27vh;
    transform: translate(136px, 0);
    opacity: 0;
  }
}
.bubble:nth-child(48) {
  background: radial-gradient(ellipse at top right, #b8c6c6 0%, #30b3d3 46%, #20628c 100%);
  width: 51px;
  height: 51px;
  left: 8vw;
  bottom: 89vh;
  -webkit-animation: move48 infinite 15s;
          animation: move48 infinite 15s;
}
@-webkit-keyframes move48 {
  0% {
    bottom: -100px;
  }
  100% {
    bottom: 34vh;
    transform: translate(106px, 0);
    opacity: 0;
  }
}
@keyframes move48 {
  0% {
    bottom: -100px;
  }
  100% {
    bottom: 34vh;
    transform: translate(106px, 0);
    opacity: 0;
  }
}
.bubble:nth-child(49) {
  background: radial-gradient(ellipse at center, #b8c6c6 0%, #30b3d3 46%, #20628c 100%);
  width: 93px;
  height: 93px;
  left: 11vw;
  bottom: 32vh;
  -webkit-animation: move49 infinite 14s;
          animation: move49 infinite 14s;
}
@-webkit-keyframes move49 {
  0% {
    bottom: -100px;
  }
  100% {
    bottom: 82vh;
    transform: translate(107px, 0);
    opacity: 0;
  }
}
@keyframes move49 {
  0% {
    bottom: -100px;
  }
  100% {
    bottom: 82vh;
    transform: translate(107px, 0);
    opacity: 0;
  }
}
.bubble:nth-child(50) {
  background: radial-gradient(ellipse at top right, #b8c6c6 0%, #30b3d3 46%, #20628c 100%);
  width: 76px;
  height: 76px;
  left: 80vw;
  bottom: 42vh;
  -webkit-animation: move50 infinite 8s;
          animation: move50 infinite 8s;
}
@-webkit-keyframes move50 {
  0% {
    bottom: -100px;
  }
  100% {
    bottom: 26vh;
    transform: translate(168px, 0);
    opacity: 0;
  }
}
@keyframes move50 {
  0% {
    bottom: -100px;
  }
  100% {
    bottom: 26vh;
    transform: translate(168px, 0);
    opacity: 0;
  }
}

```

## Credits
- [Random moving bubbles (codepen.io)](https://codepen.io/bajzarpa/pen/woYNXp)

## Backlinks 
- [[developer/CSS/CSS\|CSS]]