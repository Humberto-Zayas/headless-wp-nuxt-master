<template>
  <section class="masthead">
    <h1>{{ tagline }}</h1>
    <h2>Site features posts and pages with WordPress content</h2>
    <p>Any content can be called in via API</p>
  </section>
</template>

<script>
export default {
  data() {
    return {
      tagline: "Headless Content"
    };
  }
};
</script>

<style lang="scss" scoped>
@import "@/assets/mixins.scss";
section.masthead {
  width: 100%;
  height: 90vh;
  overflow: hidden;
  text-align: center;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  padding: 7vw;
  background: -moz-radial-gradient(
      center,
      ellipse cover,
      rgba(0, 0, 0, 0) 0%,
      rgba(0, 0, 0, 0) 37%,
      rgba(0, 0, 0, 0.65) 100%
    ),
    url("/mountains-masthead.jpg") no-repeat center center scroll; /* FF3.6-15 */
  background: -webkit-radial-gradient(
      center,
      ellipse cover,
      rgba(0, 0, 0, 0) 0%,
      rgba(0, 0, 0, 0) 37%,
      rgba(0, 0, 0, 0.65) 100%
    ),
    url("/mountains-masthead.jpg") no-repeat center center scroll; /* Chrome10-25,Safari5.1-6 */
  background: radial-gradient(
      ellipse at center,
      rgba(0, 0, 0, 0) 0%,
      rgba(0, 0, 0, 0) 37%,
      rgba(0, 0, 0, 0.65) 100%
    ),
    url("/mountains-masthead.jpg") no-repeat center center scroll; /* W3C, IE10+, FF16+, Chrome26+, Opera12+, Safari7+ */
  background-size: cover;
  color: #333;
  h1 {
    color: white;
    @include fluid-type(font-size, 320px, 1366px, 30px, 65px);
  }
  h2 {
    color: white;
  }
  p {
    color: white;
  }
}
</style>