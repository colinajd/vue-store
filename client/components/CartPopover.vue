<template>
  <div class="popover">
    <div class="cart-header">
      <button @click="toggleCart" class="delete"></button>
    </div>
    <div class="cart-container">
      <article v-for="item in getCart" class="media cart-item">
        <div class="media-left">
          <figure class="image">
            <img :src="item.image" alt="Image">
          </figure>
        </div>
        <div class="media-content">
          <div class="content">
            <div class="name-row">
              <p>
                <strong>{{ item.name }}</strong>
              </p>
              <span class="tag is-success is-small">${{ item.price }}</span>
            </div>
            <div class="name-row">
              <a @click="$store.dispatch('removeFromCart', item.id)" class="button is-danger is-small is-outlined">Remove</a>
              <div class="quantity-label">Qty: <input class="input quantity-input" type="number" @change="(e) => quantityChange(e, item)" :value="item.quantity" min="1" max="99" number /></div>
            </div>
          </div>
        </div>
      </article>
      <div v-if="getCart.length === 0" class="no-items">
        <p>No items in your cart</p>
      </div>
    </div>
    <div class="cart-footer">
      <router-link :to="{ name: 'checkout' }" class="button is-info is-small">Checkout</router-link>
    </div>
  </div>
</template>

<script>
  export default {
    props: ['toggleCart'],
    computed: {
      getCart() {
        return this.$store.getters.cart
      }
    },
    methods: {
      quantityChange(e, item) {
        const quantity = Number(e.target.value)

        if (quantity === 0) return

        const updated = item

        updated.quantity = quantity
        this.$store.dispatch('updateCartItem', updated)
      }
    }
  }
</script>

<style scoped>
  .name-row {
    display: flex;
    flex-direction: row;
    justify-content: space-between;
    align-items: center;
  }

  .name-row > p {
    font-size: 14px;
    word-wrap: break-word;
    margin-bottom: 7px;
  }

  .name-row > a {
    height: 25px;
  }

  .image > img {
    width: 64px;
    height: 52px;
  }

  .quantity-label {
    font-size: 12px;
    line-height: 23px;
    color: grey;
  }

  .quantity-input {
    height: 25px;
    width: 50px;
    font-size: 12px;
  }

  .delete {
    float: right;
    margin-top: 4px;
    margin-right: 5px;
  }

  .cart-footer {
    position: absolute;
    height: 50px;
    bottom: 0;
    background-color: #f5f5f5;
    width: 100%;
    border-bottom-left-radius: 10px;
    border-bottom-right-radius: 10px;
  }

  .cart-header {
    position: absolute;
    height: 30px;
    top: 0;
    background-color: #f5f5f5;
    width: 100%;
    border-top-left-radius: 10px;
    border-top-right-radius: 10px;
  }

  .cart-footer > a {
    margin-top: 11px;
    width: 90%;
  }

  .tag {
    align-self: flex-start;
    user-select: none;
  }

  .cart-container {
    width: 100%;
    height: 74%;
    margin-top: 30px;
    overflow-y: auto;
    overflow-x: hidden;
  }

  .no-items {
    height: 100%;
    width: 100%;
    font-size: 14px;
    display: flex;
    justify-content: center;
    align-items: center;
  }

  .content > a {
    height: 20px;
  }

  .cart-item {
    margin-left: 20px;
    margin-top: 20px;
    margin-right: 20px;
    margin-bottom: 20px;
  }

  .popover {
    position: absolute;
    width: 300px;
    height: 300px;
    background-color: white;
    border-radius: 10px;

    z-index: 10;
    -webkit-box-shadow: 1px 1px 16px -4px rgba(0,0,0,0.75);
    -moz-box-shadow: 1px 1px 16px -4px rgba(0,0,0,0.75);
    box-shadow: 1px 1px 16px -4px rgba(0,0,0,0.75);

    animation-duration: 0.4s;
    animation-fill-mode: both;
    animation-name: fadeIn;
  }

  @media only screen and (max-width: 768px) {
    /* Mobile */
    .popover {
      width: 90%;
    }
  }
</style>
