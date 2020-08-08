<template>
  <div>
    <b-navbar type="light" variant="light">
      <b-navbar-nav tabs>
        <b-nav-item active disabled><b>Fancy Quiz App</b></b-nav-item>
        <b-nav-item disabled>Counter : {{ correct }}/{{ total }}</b-nav-item>
      </b-navbar-nav>
    </b-navbar>
  </div>
</template>

<script>
export default {
  props: {
    correct: Number,
    total: Number,
  },
};
</script>
