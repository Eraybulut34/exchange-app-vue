<template>
  <div>
    <h1 class="headText mt-50rem">150 para birimine anında dönüştürün!</h1>
    <b-tabs content-class="mt-3 w-100">
      <b-tab title="Hesaplama" class="accounting" active>
        <input
          type="text"
          v-model="baseAmount"
          class="form-control"
          placeholder="Bir miktar giriniz..."
          required
        />
        <select class="form-control" v-model="baseRate">
          <option v-for="(item, index) in rates.rates" :key="index" :value="item">
            {{ index }} = {{item}}
          </option>
        </select>
        <select class="form-control" v-model="reqRate">
          <option v-for="(item, index) in rates.rates" :key="index" :value="item">
            {{ index }} = {{item}}
          </option>
        </select>

        <button @click="getResult">Hesapla</button>
        <h1 v-if="result" class="resultStyle">Sonuç : {{ result }}</h1>
      </b-tab>
      <b-tab title="Haberler">
        <b-row >
        
          <b-col v-for="new1 in news" :key="new1" md="4" mx-auto>
          <b-card class="w-250" :title="new1.title" :img-src="new1.urlToImage">
            <b-card-text>
              {{ new1.description }}
            </b-card-text>
            <b-button :href="new1.url" variant="primary" class="btn"
              >Haberi Gör</b-button
            >
          </b-card></b-col>
      </b-row>
      </b-tab>
    </b-tabs>
  </div>
</template>


<script>
export default {
  components: {},
  data() {
    return {
      news: null,
      rates: [],
      baseRate: null,
      reqRate: null,
      baseProcessRate: null,
      reqProcessRate: null,
      result: null,
      baseAmount: null,
      reqAmount: null,
      baseProcessAmount: null,
    }
  },
  async beforeMount() {
    let resp = await this.$axios.get(
      'http://api.exchangeratesapi.io/v1/latest?access_key=cb94507b17b167c53ef1dbf732a9228f'
    )
    this.rates = resp.data

    this.getNews();
  },
  computed: {},

  methods: {
    getResult() {
      this.result =
        Number(this.baseProcessAmount) * Number(this.reqRate) /
        Number(this.baseRate);
    },
    getNews() {
      this.$axios
        .get(
          'https://newsapi.org/v2/everything?q=Economy&from=2021-12-19&sortBy=popularity&apiKey=19a98ef2b69a4477bb8f4b7ce150b708'
        )
        .then((response) => (this.news = response.data.articles))
    },
  },
  watch: {
    baseAmount(newBase) {
      this.baseProcessAmount = this.baseAmount
    },
    baseRate(newBase) {
      this.baseProcessRate = this.rates.rates[newBase]
    },
    reqRate(newBase) {
      this.reqProcessRate = this.rates.rates[newBase]
    },
  },
}
</script>

<style>
.headText {
  font-size: 35px;
  color: beige;
  align-items: center;
  margin-left: 250px;
  margin-bottom: 50px;
}
.resultStyle {
  color: wheat;
  margin-bottom: 50px;
}
</style>
