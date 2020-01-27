<template>
  <div class="SpinnerBlock">
    <div class="Spinner">
      <div>
        <div></div>
      </div>
      <div>
        <div></div>
      </div>
      <div>
        <div></div>
      </div>
      <div>
        <div></div>
      </div>
      <div>
        <div></div>
      </div>
      <div>
        <div></div>
      </div>
      <div>
        <div></div>
      </div>
      <div>
        <div></div>
      </div>
      <div>
        <div></div>
      </div>
      <div>
        <div></div>
      </div>
      <div>
        <div></div>
      </div>
      <div>
        <div></div>
      </div>
    </div>
  </div>
</template>

<style lang="scss" scoped>
.SpinnerBlock {
  width: 200px;
  height: 200px;
  display: inline-block;
  overflow: hidden;
  background: #ffffff;

  .Spinner {
    width: 100%;
    height: 100%;
    position: relative;
    transform: translateZ(0) scale(1);
    backface-visibility: hidden;
    transform-origin: 0 0;
  }

  @keyframes Spinner {
    0% {
      opacity: 1;
      backface-visibility: hidden;
      transform: translateZ(0) scale(2.19, 2.19);
    }
    100% {
      opacity: 0;
      backface-visibility: hidden;
      transform: translateZ(0) scale(1, 1);
    }
  }
  .Spinner div > div {
    position: absolute;
    width: 4px;
    height: 4px;
    border-radius: 50%;
    background: #83838a;
    animation: Spinner 1s linear infinite;
  }
  .Spinner div:nth-child(1) > div {
    left: 132px;
    top: 98px;
    animation-delay: -0.9166666666666666s;
  }
  .Spinner > div:nth-child(1) {
    transform: rotate(0deg);
    transform-origin: 134px 100px;
  }
  .Spinner div:nth-child(2) > div {
    left: 127px;
    top: 115px;
    animation-delay: -0.8333333333333334s;
  }
  .Spinner > div:nth-child(2) {
    transform: rotate(30deg);
    transform-origin: 129px 117px;
  }
  .Spinner div:nth-child(3) > div {
    left: 115px;
    top: 127px;
    animation-delay: -0.75s;
  }
  .Spinner > div:nth-child(3) {
    transform: rotate(60deg);
    transform-origin: 117px 129px;
  }
  .Spinner div:nth-child(4) > div {
    left: 98px;
    top: 132px;
    animation-delay: -0.6666666666666666s;
  }
  .Spinner > div:nth-child(4) {
    transform: rotate(90deg);
    transform-origin: 100px 134px;
  }
  .Spinner div:nth-child(5) > div {
    left: 81px;
    top: 127px;
    animation-delay: -0.5833333333333334s;
  }
  .Spinner > div:nth-child(5) {
    transform: rotate(120deg);
    transform-origin: 83px 129px;
  }
  .Spinner div:nth-child(6) > div {
    left: 69px;
    top: 115px;
    animation-delay: -0.5s;
  }
  .Spinner > div:nth-child(6) {
    transform: rotate(150deg);
    transform-origin: 71px 117px;
  }
  .Spinner div:nth-child(7) > div {
    left: 64px;
    top: 98px;
    animation-delay: -0.4166666666666667s;
  }
  .Spinner > div:nth-child(7) {
    transform: rotate(180deg);
    transform-origin: 66px 100px;
  }
  .Spinner div:nth-child(8) > div {
    left: 69px;
    top: 81px;
    animation-delay: -0.3333333333333333s;
  }
  .Spinner > div:nth-child(8) {
    transform: rotate(210deg);
    transform-origin: 71px 83px;
  }
  .Spinner div:nth-child(9) > div {
    left: 81px;
    top: 69px;
    animation-delay: -0.25s;
  }
  .Spinner > div:nth-child(9) {
    transform: rotate(240deg);
    transform-origin: 83px 71px;
  }
  .Spinner div:nth-child(10) > div {
    left: 98px;
    top: 64px;
    animation-delay: -0.16666666666666666s;
  }
  .Spinner > div:nth-child(10) {
    transform: rotate(270deg);
    transform-origin: 100px 66px;
  }
  .Spinner div:nth-child(11) > div {
    left: 115px;
    top: 69px;
    animation-delay: -0.08333333333333333s;
  }
  .Spinner > div:nth-child(11) {
    transform: rotate(300deg);
    transform-origin: 117px 71px;
  }
  .Spinner div:nth-child(12) > div {
    left: 127px;
    top: 81px;
    animation-delay: 0s;
  }
  .Spinner > div:nth-child(12) {
    transform: rotate(330deg);
    transform-origin: 129px 83px;
  }

  .Spinner div {
    box-sizing: content-box;
  }
}
</style>