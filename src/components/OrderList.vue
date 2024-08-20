<template>
    <div class="order-list">
        <h2>Your Order from <br> MyGroceries</h2>
        <ul>
            <li v-for="(item, index) in orders" :key="index" class="order-item">
                <div class="item-details">
                    <span class="item-name">{{ item.name }}</span>
                    <span class="item-quantity">x{{ item.quantity }}</span>
                </div>
                <div class="item-price">{{ formatPrice(item.price) }}</div>
            </li>
        </ul>
        <div class="order-total">
            <span>Total:</span>
            <span>{{ formatPrice(totalPrice) }}</span>
        </div>
    </div>
</template>

<script>
export default {
    name: 'OrderList',
    props: {
        orders: {
            type: Array,
            required: true,
            default: () => [
                { name: 'Anchor', quantity: 1, price: 15.42 },
                { name: 'Bread', quantity: 2, price: 2.50 },
                { name: 'Cheese', quantity: 1, price: 3.00 }
            ],
        },
    },
    computed: {
        totalPrice() {
            return this.orders.reduce((total, item) => {
                return total + item.price * item.quantity;
            }, 0);
        },
    },
    methods: {
        formatPrice(value) {
            return `RM ${value.toFixed(2)}`;
        },
    },
};
</script>

<style scoped>
.order-list {
    width: 20vw;
    height: auto;   
    /* Adjust the width to match the desired size */
    border: 1px solid #ccc;
    padding: 15px;
    border-radius: 8px;
    box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.1), 0 6px 20px 0 rgba(0, 0, 0, 0.1);
    background-color: #ffffff;
}

.order-list h2 {
    margin-bottom: 4vh;
    font-size: 1.2em;
    font-family: Arial, sans-serif;
    font-weight: bold;
    color: #333;
    text-align: left;
}

ul {
    padding-left: 0;
    list-style-type: none;
    margin: 0;
}

.order-item {
    display: flex;
    justify-content: space-between;
    margin-bottom: 8px;
    font-size: 0.9em;
    font-family: Arial, sans-serif;
    color: #555;
    align-items: center; /* Ensure vertical alignment of text */
}

.item-details {
    flex: 1; /* Allow item details to take up available space */
    display: flex;
    justify-content: space-between;
    margin-right: 10px; /* Add space between the item details and the price */
}

.item-name {
    flex: 1;
    font-weight: normal;
    overflow: hidden;
    text-overflow: ellipsis; /* Add ellipsis if the text is too long */
    white-space: nowrap; /* Prevent text from wrapping to a new line */
}

.item-quantity {
    font-weight: bold;
    margin-left: 10px;
}

.item-price {
    flex-shrink: 0; /* Prevent the price from shrinking */
}

.order-total {
    display: flex;
    justify-content: space-between;
    margin-top: 15px;
    font-size: 1em;
    font-weight: bold;
    border-top: 1px solid #ccc;
    padding-top: 10px;
    color: #000;
}
</style>