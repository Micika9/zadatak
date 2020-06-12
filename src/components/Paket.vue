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
      return this.formatMoney(this.paket.prices.old_price_recurring[this.contractLength]) || null
    },

    price () {
      return this.formatMoney(this.paket.prices.price_recurring[this.contractLength])
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
