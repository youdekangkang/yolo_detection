<template>
  <div class="header">
    <div class="header-title">
      <h1>{{ msg }}</h1>
    </div>
  </div>
</template>


<script>
export default {
  name: "Header",
  data() {
    return {
      msg: "这是系统标题",
      activeIndex: "1",
    };
  },
  methods: {},
};
</script>
<style scoped>
.header {
  height: 100%;
  display: flex;
  justify-content: center;
  color: #21b3b9;
  font-size: 20px;
  align-items: center;
}
.header-title {
  //color: red;
}
</style>


