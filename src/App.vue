<template>
  <div id="app">
    <!-- <img src="./assets/logo.png"> -->
    <h1>Welcome to the one and only Online Market</h1>

    <div class="container">
      <!-- Display any errors -->
      <div v-if="showApiError" class="error-box">{{ apiError }}</div>

      <!-- Show Products -->
      <div class="model-box">
        <div class="product-details">
          <!-- Verdict -->
          <div class="model-verdict"><span class="quotes">&quot;</span>{{ modelInfo.verdict }}<span class="quotes">&quot;</span></div>

          <!-- Product Inner Information -->
          <div class="product-inner-wrapper">
            <div class="model-info">
              <span class="model-text">{{ modelInfo.name }}</span>
            </div>
            
            <!-- Model Image -->
            <div class="model-image"><img :src="modelInfo.image_url" alt="iPad Air model 8129" ></div>

            <!-- Prices -->
            <div class="prices">
              <ul>
                <li v-for="(price, currency) in modelInfo.prices">
                  {{ currency }} : {{ price }}
                </li>
              </ul>
            </div>

            <!-- Pros and Cons -->
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

            <!-- Reviews -->
            <div class="reviews">
              <button @click="showProductDeals" type="button" class="btn btn-primary">{{ showDealsButtonText }}</button>
              <a class="btn btn-link" :href="modelInfo.review_link" target="__blank">Product Reviews</a>
            </div>
          </div><!-- end product inner wrapper -->
          
          <!-- Show list of products -->
          <div v-if="showProducts" class="product-list-box">
            <h1>Deals</h1>

            <div v-for="deal in offers" class="product-list">
              <table>
                <tr>
                  <!-- <td>{{ deal.merchant.name }}</td> -->
                  <td><img :src="deal.merchant.logo_url" alt="product image"></td>
                  <td>{{ deal.offer.name }}</td>
                  <td><span v-html="deal.offer.currency_symbol" class="m-r-2"></span>{{ deal.offer.price }}</td>
                  <td><a class="btn-view-deal" :href="deal.offer.link" target="__blank">View</a></td>
                </tr>
              </table>
            </div>
          </div>
        </div><!-- end product details -->
      </div><!-- end model box -->
    </div><!-- end container -->
  </div><!-- end app -->
</template>

<script>
export default {
  name: 'app',
  
  /*
    Runs when the app is initialised
  */
  mounted () {
    this.axios.get(this.apiUrl)
      .then((response) => {
        this.modelInfo = response.data.widget.data.model_info['8129']
        this.offers = response.data.widget.data.offers
      },
      (error) => {
        this.apiError = "Error: " + error.message + ". Could not get required data, please try again later."
        this.showApiError = true
      });
  },
  /*
    Stores all app data
  */
  data () {
    return {
      apiUrl: 'http://search-api.fie.future.net.uk/widget.php?id=review&site=TRD&model_name=iPad_Air',
      modelInfo: {},
      offers: {},
      showApiError: false,
      apiError: "",
      showProducts: false,
      showDealsButtonText: "Show Latest Deals",
    }
  },

  /*
    Defines all methods used in the app
  */
  methods: {
    
    // When user clicks on Show Latest Deals Button
    showProductDeals () {
      // Show/Hide products
      this.showProducts = !this.showProducts

      // Change text of button accordingly
      if (this.showProducts) {
        this.showDealsButtonText = "Hide Deals"
      } else {
        this.showDealsButtonText = "Show Latest Deals"
      }
    }
  }
}
</script>

<!-- SASS -->
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
table {
  table-layout: fixed;
  width: 100%;
}
.m-r-2 {
  margin-right: 5px;
}
.btn {
  width: 200px;
  border: none;
  padding: 20px;
  border-radius: 5px;
  color: #fff;
  font-size: 1rem;
  font-weight: bold;
  cursor: pointer;
  &:focus {
    outline: none;
  }
  -webkit-transition: all 0.3s ease;
	   -moz-transition: all 0.3s ease;
	    -ms-transition: all 0.3s ease;
	     -o-transition: all 0.3s ease;
	        transition: all 0.3s ease;
}
.btn-primary {
  background-color: #00cbef;
  &:hover {
    background-color: #03778c;
  }
}
.btn-link {
  background-color: #fdd62a;
  &:hover {
    background-color: #e9be00;
  }
}
.btn-view-deal {
  // width: 200px;
  border: none;
  padding: 8px 35px;
  border-radius: none;
  color: #fff;
  font-size: 0.8rem;
  font-weight: bold;
  cursor: pointer;
  background-color: #08c702;
  &:hover {
    background-color: rgb(28, 170, 0);
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
.model-box {
  width: 60%;
  margin: 30px auto;
}
.product-details {
  background-color: #eee;
  padding: 30px;
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
  .product-inner-wrapper {
    background-color: #fff;
    padding: 40px;
    width: 80%;
    margin: 20px auto;
    .model-info {
      margin-bottom: 20px;
    }
    .model-text {
      font-size: 1.4rem;
    }
    .pros-cons {
      border: 1px dashed #ccc;
      border-radius: 5px;
      padding: 20px;
      overflow: auto;
      width: 400px;
      margin: 20px auto 50px auto;
      .pros {
        float: left;
        li {
          color: #42b983;
        }
      }
      .cons {
        float: right;
        border-left: 1px solid #ccc;
        padding-left: 18px;
        li {
          color: crimson;
        }
      }
    }
    .reviews {
      a {
        color: #505050;
      }
    }
  }

  .product-list-box {
    
    .product-list {
      background-color: #fff;
      width: 80%;
      margin: 20px auto;
      padding: 40px;
    }
  }
}
</style>
