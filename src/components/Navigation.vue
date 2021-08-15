<template>
  <div class="text-center">
    <ul class="nav bg-red justify-content-center position-fixed w-100 p-2 top-0">
      <li class="nav-item">
          <a class="nav-link" :class="{ active: isActive[0] }" @click="handleClick(0)">Thống kê</a>
      </li>
      <li class="nav-item">
          <a class="nav-link" :class="{ active: isActive[1] }" @click="handleClick(1)">Cứu trợ</a>
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  name: "Resume",
  props: {
    isActive: Array
  },
  methods: {
    handleClick(index){
      this.$emit('handleActive', index)
    }
  }
};
</script>

<style>
  .nav-link {
    color: yellow !important;
    font-weight: 500;
  }

  .active {
    color: #fff !important;
    font-weight: 800;
  }

  .bg-red {
    background-color: red;
  }

  .nav {
    z-index: 2;
  }
</style>