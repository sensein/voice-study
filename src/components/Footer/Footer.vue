<template>
  <footer>
    <div>
      voice-up is a demo and not an actual study.
    </div>
  </footer>
</template>

<script>
export default {
  name: 'Footer',
};
</script>

<style scoped>
footer {
  align-items: baseline;
  background-color: #219c6d;
  color: black;
  display: flex;
  flex-wrap: wrap;
  font-size: .6rem;
  justify-content: space-around;
  padding: .25rem .75rem .5rem .75rem;
  position: fixed;
  left: 0;
  bottom: 0;
  width: 100%;
}
footer > div {
  display: flex;
  align-items: baseline;
  justify-content: space-around;
  text-align: center;
}
a {
  color: black;
}
footer > div:first-child > * {
  padding: .5vw;
  line-height: 1.3;
}
@media screen and (max-width: 50em) {
  footer {
    font-size: 3vw;
  }
}
</style>
