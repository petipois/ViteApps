<template>
  <div class="popup">
    <div class="popup-inner">
      <slot/>
      <button class="popup-close">Close</button>
    </div>
  </div>
</template>

<script>
export default {
};
</script>

<style>
.popup {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  z-index: 99;
  background-color: rgba(0, 0, 0, 0.6);
  display: flex;
  align-items: center;
  justify-content: center;
}

.popup-inner {
  background: #fff;
  padding: 32px;
}
</style>
