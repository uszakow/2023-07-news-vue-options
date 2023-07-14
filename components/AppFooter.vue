<template>
  <footer class="footer text-center py-1">
    <span>Created by</span>&nbsp;
    <a
      href="https://www.linkedin.com/in/p-uszakow/"
      target="_blank"
      rel="author"
    >
      Paweł Uszakow
    </a>
  </footer>
</template>

<style lang="scss" scoped>
@import "styles/variables";

.footer {
  font-style: italic;
  color: $dark-grey;
  background-color: $background;

  a {
    position: relative;
  }

  a::after {
    content: "";
    width: 100%;
    position: absolute;
    bottom: -1px;
    left: 0;
    border: 1px solid $dark-grey;
    transform: scaleX(0);
    transform-origin: bottom right;
    transition: transform 0.4s ease-in-out;
  }

  a:hover::after {
    transform: scaleX(1);
    transform-origin: bottom left;
  }
}
</style>
