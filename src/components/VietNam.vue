<template>
  <div class="row">
    <div class="col-12 mx-auto">
    <div class="text-center">
      <h2>Việt Nam </h2>
    </div>
    <hr>
    </div>
    <div class="col-md-3 mx-auto">
      <div class="card border-secondary mb-3">
        <div class="card-header text-secondary">
          <div class="row">
            <div class="col-9">Tổng số ca</div>
            <img src="../assets/vn.png" class="col-3 vn-flag" alt="Vietnam flag">
          </div>
        </div>
        <div class="card-body">
          <h5 class="card-title text-center">{{ String(vn_confirmed).replace(/(.)(?=(\d{3})+$)/g,'$1,') }}</h5>
        </div>
      </div>
    </div>
    <div class="col-md-3 mx-auto">
      <div class="card border-success mb-3">
        <div class="card-header text-success">
          <div class="row">
            <div class="col-9">Đang điều trị</div>
            <img src="../assets/vn.png" class="col-3 vn-flag" alt="Vietnam flag">
          </div>
        </div>
        <div class="card-body">
          <h5 class="card-title text-center">{{ String(vn_recovered).replace(/(.)(?=(\d{3})+$)/g,'$1,') }}</h5>
        </div>
      </div>
    </div>
    <div class="col-md-3 mx-auto">
      <div class="card border-danger mb-3">
        <div class="card-header text-danger">
          <div class="row">
            <div class="col-9">Tử vong</div>
            <img src="../assets/vn.png" class="col-3 vn-flag" alt="Vietnam flag">
          </div>
        </div>
        <div class="card-body">
          <h5 class="card-title text-center">{{ String(vn_deaths).replace(/(.)(?=(\d{3})+$)/g,'$1,') }}</h5>
        </div>
      </div>
    </div>
    <div class="col-md-3 mx-auto">
      <div class="card border-info mb-3">
        <div class="card-header text-info">
          <div class="row">
            <div class="col-9">Tỉ lệ tử vong</div>
            <img src="../assets/vn.png" class="col-3 vn-flag" alt="Vietnam flag">
          </div>
        </div>
        <div class="card-body">
          <h5 class="card-title text-center">{{ (vn_deaths/vn_confirmed*100).toFixed(2) }}%</h5>
        </div>
      </div>
    </div>
    <div class="col-md-4 mx-auto">
      <div class="card border-primary mb-3">
        <div class="card-header text-primary">
          <div class="row">
            <div class="col-10">Tổng số người tiêm vacine</div>
            <img src="../assets/vn.png" class="col-2 vn-flag" alt="Vietnam flag">
          </div>
        </div>
        <div class="card-body">
          <h5 class="card-title text-center">{{ String(vn_administered).replace(/(.)(?=(\d{3})+$)/g,'$1,') }}</h5>
        </div>
      </div>
    </div>
    <div class="col-md-4 mx-auto">
      <div class="card border-primary mb-3">
        <div class="card-header text-primary">
          <div class="row">
            <div class="col-10">Đã tiêm 2 mũi</div>
            <img src="../assets/vn.png" class="col-2 vn-flag" alt="Vietnam flag">
          </div>
        </div>
        <div class="card-body">
          <h5 class="card-title text-center">{{ String(vn_people_vaccinated).replace(/(.)(?=(\d{3})+$)/g,'$1,') }}</h5>
        </div>
      </div>
    </div>
    <div class="col-md-4 mx-auto">
      <div class="card border-primary mb-3">
        <div class="card-header text-primary">
          <div class="row">
            <div class="col-10">Đã tiêm mũi đầu tiên</div>
            <img src="../assets/vn.png" class="col-2 vn-flag" alt="Vietnam flag">
          </div>
        </div>
        <div class="card-body">
          <h5 class="card-title text-center">{{ String(vn_people_partially_vaccinated).replace(/(.)(?=(\d{3})+$)/g,'$1,') }}</h5>
        </div>
      </div>
    </div>
    <div class="col-12 text-center" data-aos="fade-right">
      <a href="https://vietnam-pandemic.web.app" class="detail">
        <img src="https://www.seekpng.com/png/full/25-254372_80-hand-drawn-arrows-ps-brush-set-arrow.png" alt="detail">
        Xem thêm thông tin dịch bệnh ở Việt Nam
      </a>
    </div>
  </div>
</template>

<script>
export default {
  name: 'VietNam',
  props: {
    vn_confirmed: Number,
    vn_recovered: Number,
    vn_deaths: Number,
    vn_administered: Number,
    vn_people_vaccinated: Number,
    vn_people_partially_vaccinated: Number
  }
}
</script>

<style scoped>
  .vn-flag {
    width: 62px;
  }
  h2 {
    margin: 30px 0 0 0;
  }
  hr{
    margin: 0 0 20px 0;
  }
  .detail {
    color: darkgreen;
    text-decoration: none;
    font-size: 16px;
  }
  .detail > img {
    width: 20px;
  }
</style>
