<script>
export default {
  name: "FooterItem",
};
</script>

<template>
  <footer class="text-white p-3">
    <div class="d-flex justify-content-around">
      <p>property of alyafimanda</p>
      <p>&copy; Al-Qur'an</p>
      <p></p>
    </div>
  </footer>
</template>

<style scoped>
footer {
  background-color: black;
}
</style>

<style scoped>
footer {
  font-family: "Dubai Medium";
  background-color: lightslategray;
  position: fixed;
  bottom: 0;
  width: 100%;
  animation: hue-rotate 2s linear infinite alternate;
}
@keyframes hue-rotate {
  to {
    filter: hue-rotate(90deg);
  }
}
</style>
