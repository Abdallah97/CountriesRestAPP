<template>
  <div class="spinner">
    <div class="circle one"></div>
    <div class="circle two"></div>
    <div class="circle three"></div>
  </div>
</template>

<script>
export default {};
</script>

<style scoped>
.spinner {
  margin: 20px auto;
  width: 500px;
  height: 450px;
  background: #fff;
  display: flex;
  justify-content: center;
  align-items: center;
  position: relative;
}

.spinner .circle {
  animation: spinners linear infinite;
  position: absolute;
  border: 5px solid transparent;
  border-top-color: #f00;
  border-radius: 50%;
}

.spinner .one {
  width: 50px;
  height: 50px;
  border-top-color: #8f3b76;
  animation-duration: 0.85s;
}

.spinner .two {
  width: 70px;
  height: 70px;
  border-top-color: #c7417b;
  animation-duration: 0.95s;
}

.spinner .three {
  width: 100px;
  height: 100px;
  border-top-color: #f5487f;
  animation-duration: 1.05s;
}

@keyframes spinners {
  0% {
    transform: rotate(0deg);
  }

  100% {
    transform: rotate(360deg);
  }
}
</style>