<template>
  <div class="row">
    <div class="col-12">
     <div class="text-center"><h2>Thế giới</h2></div>
      <hr>
    </div>
    <div class="col-md-3 mx-auto">
      <div class="card border-secondary mb-3">
        <div class="card-header text-secondary">Tổng số ca</div>
        <div class="card-body">
          <h5 class="card-title text-center">{{ String(confirmed).replace(/(.)(?=(\d{3})+$)/g,'$1,') }}</h5>
        </div>
      </div>
    </div>
    <div class="col-md-3 mx-auto">
      <div class="card border-success mb-3">
        <div class="card-header text-success">Đang điều trị</div>
        <div class="card-body">
          <h5 class="card-title text-center">{{ String(recovered).replace(/(.)(?=(\d{3})+$)/g,'$1,') }}</h5>
        </div>
      </div>
    </div>
    <div class="col-md-3 mx-auto">
      <div class="card border-danger mb-3">
        <div class="card-header text-danger">Tử vong</div>
        <div class="card-body">
          <h5 class="card-title text-center">{{ String(deaths).replace(/(.)(?=(\d{3})+$)/g,'$1,') }}</h5>
        </div>
      </div>
    </div>
    <div class="col-md-3 mx-auto">
      <div class="card border-info mb-3">
        <div class="card-header text-info">Tỉ lệ tử vong</div>
        <div class="card-body">
          <h5 class="card-title text-center">{{ (deaths/confirmed*100).toFixed(2) }}%</h5>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'HomePage',
  props: {
    confirmed: Number,
    recovered: Number,
    deaths: Number
  }
}
</script>

<style scoped>
  h2 {
    margin: 30px 0 0 0;
  }
  hr{
    margin: 0 0 20px 0;
  }
</style>
