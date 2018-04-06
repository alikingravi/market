<template>
  <div id="app">
    <!-- <img src="./assets/logo.png"> -->
    <h1>ONLINE MARKET</h1>
    <h1>Welcome to the one and only Online Market</h1>
    <h2>Click on the button below to get all the latest deals!</h2>
    
    <button @click="getApiData" type="button" class="btn">Get Latest Deals on iPad Air</button>

    <div class="container">
      <!-- Display any errors -->
      <div v-if="showApiError" class="error-box">{{ apiError }}</div>

      <!-- Show Products -->
      <div v-if="showProducts" class="product-box">
        <div class="product-details">
          <h1>Model Information</h1>
          <div class="model-info">
            <span class="model-title">Model Name:</span>
            <span class="model-text">{{ modelInfo.name }}</span>
          </div>
          
          <div class="model-verdict"><span class="quotes">&quot;</span>{{ modelInfo.verdict }}<span class="quotes">&quot;</span></div>
          <div class="model-image"><img :src="modelInfo.image_url" alt="iPad Air model 8129"></div>
          <div class="pros-cons">
            <span class="pros">
              <h3>Pros</h3>
              <ul>
                <li v-for="pros in modelInfo.pros">{{ pros }}</li>
              </ul>
            </span>
            <span class="cons">
              <h3>Cons</h3>
              <ul>
                <li v-for="cons in modelInfo.cons">{{ cons }}</li>
              </ul>
            </span>
          </div>
          <div class="prices">
            <h3>Prices:</h3>
            <ul>
              <li v-for="(price, currency) in modelInfo.prices">
                {{ currency }} : {{ price }}
              </li>
            </ul>
          </div>

          <div class="reviews">
            <a :href="modelInfo.review_link" target="__blank">Click here for product reviews</a>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'app',
  mounted () {
    this.axios.get(this.apiUrl)
      .then((response) => {
        // console.log(response.data.widget.data)

        this.modelInfo = response.data.widget.data.model_info['8129']
      },
      (error) => {
        this.apiError = "Error: " + error.message + ". Could not get required data, please try again later."
        this.showApiError = true
      });
  },
  data () {
    return {
      apiUrl: 'http://search-api.fie.future.net.uk/widget.php?id=review&site=TRD&model_name=iPad_Air',
      allData: {},
      modelInfo: {},
      showApiError: false,
      apiError: "",
      showProducts: true,
    }
  },
  methods: {
    getApiData () {
      
    }
  }
}
</script>

<style lang="scss">
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
h1, h2 {
  font-weight: normal;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  margin: 10px 0px;
}
a {
  color: #42b983;
}
.btn {
  height: 50px;
  width: 200px;
  border: none;
  background-color: #00cbef;
  border-radius: 5px;
  color: #fff;
  font-size: 1rem;
  font-weight: bold;
  cursor: pointer;
  &:hover {
    background-color: #03778c;
  }
  &:focus {
    outline: none;
  }
  -webkit-transition: all 0.3s ease;
	   -moz-transition: all 0.3s ease;
	    -ms-transition: all 0.3s ease;
	     -o-transition: all 0.3s ease;
	        transition: all 0.3s ease;
}

.error-box {
  width: 30%;
  background-color: rgba(255, 0, 0, 0.3);
  padding: 30px;
  margin: 30px auto;
  color: #fff;
}

.product-box {
  width: 80%;
  
  // border: 1px solid #ccc;
  // border-radius: 5px;
  // background-color: #eee;
  margin: 30px auto;
}
.product-details {
  background-color: #eee;
  padding: 30px;

  .model-info {
    margin-bottom: 20px;
  }
  .model-title {
    font-size: 1.4rem;
  }
  .model-text {
    font-size: 1.4rem;
  }
  .model-verdict {
    width: 700px;
    margin: 20px auto;
    font-size: 1.2rem;
    font-style: italic;
    line-height: 2.2rem;
    .quotes {
      font-size: 2rem;
    }
  }
}
</style>
