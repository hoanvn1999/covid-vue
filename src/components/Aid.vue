<template>
  <div class="row">
    <div class="col-12 mx-auto">
      <div class="text-center">
        <h2>Thông tin cứu trợ</h2>
      </div>
      <hr>
    </div>
    <form>
      <div class="form-group row">
        <input type="text" class="form-control col-4" ref="name" placeholder="Họ và tên">
        <input type="text" class="form-control col-4" ref="phone" placeholder="Số điện thoại">
        <input type="text" class="form-control col-4" ref="address" placeholder="Địa chỉ">
      </div>
      <div class="form-group">
        <label for="status">Tình trạng</label>
        <textarea class="form-control" id="status" ref="status" rows="3"></textarea>
      </div>
    </form>
    <table class="table table-striped">
      <thead class="bg-primary text-white">
        <tr>
          <th scope="col" class="text-center">Họ và tên</th>
          <th scope="col" class="text-center">Số điện thoại</th>
          <th scope="col" class="text-center">Địa chỉ</th>
          <th scope="col" class="text-center">Tình trạng</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="victim in victims" :key="victim.name">
          <th scope="row" class="text-center">{{ victim.name }}</th>
          <th scope="row" class="text-center">{{ victim.phone }}</th>
          <th scope="row" class="text-center">{{ victim.address }}</th>
          <th scope="row" class="text-center">{{ victim.status }}</th>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
import db from "../firebase/firebaseInit";

export default {
  name: 'Aid',
  data() {
    return {
      vitims: [],
    };
  },
	methods: {
		getVictims() {
			db.collection("vitims").get().then(snap => {
        snap.forEach(res => {
          this.vitims.push(res.data())
        })
      });
		}
	},
  created() {
		this.getVictims();
  },
}
</script>

<style scoped>
</style>
