<template>
  <div class="product">
    <div
      v-if="paket.is_featured"
      class="featured">
      <p>{{ data.promo_text }}</p>
    </div>
    <div class="text">
      <h2>{{ paket.name }}</h2>
    </div>
    <div class="tv">
      <img :src="data.assets.tv_category">
      <ul>
        <li
          v-for="item in tvCategoryItems"
          :key="item.id">
          {{ item.long_name }}
        </li>
      </ul>
    </div>
    <div class="net">
      <img :src="data.assets.net_category">
      <ul>
        <li
          v-for="item in netCategoryItems"
          :key="item.id">
          {{ item.long_name }}
        </li>
      </ul>
    </div>
    <div class="promotions">
      <ul>
        <template v-for="promotion in paket.promotions">
          <div v-if="showPromotion(promotion)" :key="promotion.id">
            <img :src="promotion.promotion_image">
            <span v-html="promotion.promo_text" />
          </div>
        </template>
      </ul>
    </div>
    <div class="price">
      <div class="old-price">
        <span v-if="oldPrice" class="old"><s>{{ oldPrice }} rsd/mes.</s></span>
        <span class="current">{{ price }} rsd/mes.</span>
      </div>
      <span class="extra" v-html="paket.prices.old_price_promo_text" />

      <button class="order">
        Naručite
      </button>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    paket: {
      type: Object,
      required: true
    },

    contractLength: {
      type: String,
      required: true
    },

    data: {
      type: Object,
      required: true
    }
  },

  computed: {
    oldPrice () {
      const price = this.paket.prices.old_price_recurring[this.contractLength]
      return price ? this.formatMoney(price) : null
    },

    price () {
      const price = this.paket.prices.price_recurring[this.contractLength]
      return price ? this.formatMoney(price) : null
    },

    tvCategoryItems () {
      return this.paket.included.filter(item => item.product_category === 'tv')
    },

    netCategoryItems () {
      return this.paket.included.filter(item => item.product_category === 'net')
    }
  },

  methods: {
    showPromotion (promotion) {
      // Checks if promotion discount variantions includes current selected contract length
      return promotion.discount_variations.includes(this.contractLength)
    },

    formatMoney (value) {
      const formatter = new Intl.NumberFormat('de-DE')
      return formatter.format(parseInt(value))
    }
  }
}
</script>

<style lang="scss" scoped>
.product {
  width: 340px;
  margin-right: 30px;
  height: 100%;
  padding: 0 15px;
  margin-top: 60px;
  background-color: #f7f3eb;
  border-radius: 10px;
  position: relative;
  .featured {
    position: absolute;
    top: -30px;
    left: 0;
    height: 50px;
    background-color: #f7f3eb;
    width: 100%;
    padding: 0 20px;
    border-top-left-radius: 10px;
    border-top-right-radius: 10px;
    p {
      display: block;
      width: 100%;
      line-height: 40px;
      border-bottom: 1px solid #d4d4d4;
      font-size: 15px;
      color: #742d6c;
      text-align: center;
    }
  }
  &:last-of-type {
    margin-right: 0;
  }
  .text {
    width: 100%;
    height: 100%;
    border-bottom: 1px solid #d3d3d3;
    h2 {
      width: 200px;
      font-size: 38px;
      line-height: 50px;
      text-align: center;
      margin: 55px auto;
      color: #742d6c;
    }
    &:first-child {
      height: 106px;
    }
  }
  .tv {
    width: 100%;
    height: 190px;
    border-bottom: 1px solid #d3d3d3;
    display: flex;
    justify-content: center;
    align-items: center;
    img {
      width: 45px;
      height: 45px;
      line-height: 40px;
    }
    ul {
      width: 100%;
      margin: 0 50px 0;
      li {
        color: #102542;
        font-size: 15px;
      }
    }
  }
  .net {
    width: 100%;
    height: 110px;
    border-bottom: 1px solid #d3d3d3;
    display: flex;
    justify-content: center;
    align-items: center;
    img {
      width: 45px;
      height: 45px;
      line-height: 40px;
    }
    ul {
      width: 100%;
      margin: 0 30px 0;
      li {
        color: #102542;
        font-size: 15px;
      }
    }
  }
  .promotions {
    background-color: #ffff;
    border-bottom: 1px solid #d3d3d3;
    img {
      width: 80px;
      height: 80px;
      line-height: 80px;
    }
    span {
      display: inline-block;
      color: #742d6c;
      font-size: 12px;
      margin: 30px 15px 0;
    }
  }
  .price {
    width: 100%;
    height: 140px;
    margin: 40px 0;
    .old-price {
      margin-bottom: 20px;
      .old {
        font-size: 18px;
        color: #742d6c;
        line-height: 30px;
      }
      .current {
        margin-left: 100px;
        font-size: 22px;
        color: #742d6c;
      }
    }
    .order {
      width: 100%;
      padding: 15px;
      margin: 30px auto;
      background-color: #742d6c;
      cursor: pointer;
      border-radius: 10px;
      color: #fff;
    }
  }
}
@media all and (max-width: 768px) {
  .product {
    margin-right: 0;
    .featured {
      margin-top: 30px;
      margin-bottom: 50px;
    }
  }
}
@media all and (max-width: 320px) {
  .product {
    width: 100%;
    .text {
      h2 {
        font-size: 30px;
      }
    }
    .tv {
      ul {
        li {
          font-size: 13px;
        }
      }
    }
    .net {
      ul {
        li {
          font-size: 13px;
        }
      }
    }
    .price {
      .old-price {
        display: flex;
        flex-direction: column-reverse;
        .old {
          font-size: 15px;
          line-height: 30px;
          text-align: center;
          display: block;
          flex-direction: row-reverse;
        }
        .current {
          margin: 0;
          font-size: 22px;
          line-height: 30px;
          text-align: center;
        }
      }
      .order {
        font-size: 14px;
      }
    }
    .promotions {
      span {
        margin: 0;
        position: absolute;
        bottom: 250px;
        margin-left: 20px;
      }
    }
  }
}
</style>
