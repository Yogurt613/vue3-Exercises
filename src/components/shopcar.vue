<template>
    <div id="app" class="container mt-4">
        <div class="d-flex justify-content-between align-items-center">
            <h2>精選商品</h2>
            <button type="button" class="btn btn-secondary" @click="showCart = true" data-bs-toggle="modal" data-bs-target="#cartModal">
                <!-- 使用 Font Awesome icon 顯示購物車 -->
                <i class="fa-solid fa-cart-shopping"></i>
            </button>
        </div>

        <!-- 商品展示區域 -->
        <div class="row row-cols-1 row-cols-md-2 row-cols-lg-4 g-4">
            <div v-for="product in products" :key="product.id">
                <div class="card">
                    <img :src="product.image" class="card-img-top" :alt="product.name" />
                    <div class="card-body">
                        <h5 class="card-title">{{ product.name }}</h5>
                        <p class="card-text">價格: NT${{ product.price }}</p>
                        <button type="button" class="btn btn-primary" @click="selectProduct(product)" data-bs-toggle="modal" data-bs-target="#productModal">
                            加入購物車
                        </button>
                    </div>
                </div>
            </div>
        </div>

        <!-- 加入購物車的 modal -->
        <div class="modal fade" id="productModal" tabindex="-1" aria-labelledby="productModalLabel" aria-hidden="true">
            <div class="modal-dialog">
                <div class="modal-content">
                    <div class="modal-header">
                        <h5 class="modal-title" id="productModalLabel">加入購物車</h5>
                        <!-- 關閉 modal 框按鈕 -->
                        <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
                    </div>
                    <div class="modal-body">
                        <!-- 顯示選中的產品名稱和價格 -->
                        <h5>{{ selectedProduct.name }}</h5>
                        <p>價格: NT${{ selectedProduct.price }}</p>
                        <!-- 輸入購買數量 -->
                        <div class="mb-3">
                            <label for="quantity" class="col-form-label">數量:</label>
                            <div class="input-group">
                                <!-- 減少數量按鈕 -->
                                <button class="btn btn-outline-secondary" type="button" @click="decreaseQuantity">-</button>
                                <!-- 顯示當前數量 -->
                                <input type="text" class="form-control text-center" v-model.number="quantity" readonly />
                                <!-- 增加數量按鈕 -->
                                <button class="btn btn-outline-secondary" type="button" @click="increaseQuantity">+</button>
                            </div>
                        </div>
                    </div>
                    <div class="modal-footer">
                        <!-- 確定按鈕 -->
                        <button type="button" class="btn btn-primary" @click="addToCart" data-bs-dismiss="modal">
                            確定
                        </button>
                        <!-- 取消按鈕 -->
                        <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">取消</button>
                    </div>
                </div>
            </div>
        </div>

        <!-- 購物車視窗 -->
        <div class="modal fade" id="cartModal" tabindex="-1" aria-labelledby="cartModalLabel" aria-hidden="true">
            <div class="modal-dialog">
                <div class="modal-content">
                    <div class="modal-header">
                        <h5 class="modal-title" id="cartModalLabel">購物車</h5>
                        <!-- 關閉視窗按鈕 -->
                        <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
                    </div>
                    <div class="modal-body">
                        <!-- 列出購物車中的每個商品 -->
                        <ul class="list-group">
                            <!-- 使用 v-for 渲染每個購物車項目 -->
                            <li v-for="item in cart" :key="item.product.id" class="list-group-item">
                                <div class="d-flex justify-content-between align-items-center">
                                    <div>
                                        <h6>{{ item.product.name }}</h6>
                                        <p>單價: NT${{ item.product.price }}</p>
                                        <!-- 顯示商品數量 -->
                                        <div class="input-group input-group-sm">
                                            <!-- 減少數量按鈕 -->
                                            <button class="btn btn-outline-secondary btn-sm" type="button" @click="decreaseQuantity(item)">-</button>
                                            <!-- 顯示當前數量 -->
                                            <input type="text" class="form-control text-center" v-model.number="item.quantity" readonly />
                                            <!-- 增加數量按鈕 -->
                                            <button class="btn btn-outline-secondary btn-sm" type="button" @click="increaseQuantity(item)">+</button>
                                        </div>
                                    </div>
                                    <!-- 移除按鈕 -->
                                    <button class="btn btn-danger btn-sm" @click="remove(item.product.id)">移除</button>
                                </div>
                            </li>
                        </ul>
                    </div>
                    <div class="modal-footer">
                        <!-- 關閉按鈕 -->
                        <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">關閉</button>
                        <!-- 結帳按鈕 -->
                        <button type="button" class="btn btn-primary" @click="checkout">結帳</button>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    data() {
        return {
            products: [
                { id: 1, name: '迷你單朵小香花束', price: 680, image: 'https://picsum.photos/seed/picsum/150/150' },
                { id: 2, name: '單隻永生玫瑰', price: 680, image: 'https://picsum.photos/seed/picsum/150/150' },
                { id: 3, name: '永生花水氧機-薰衣草紫', price: 680, image: 'https://picsum.photos/seed/picsum/150/150' },
                { id: 4, name: '單隻永生玫瑰', price: 680, image: 'https://picsum.photos/seed/picsum/150/150' },
                { id: 5, name: '永生花水氧機-薰衣草紫', price: 680, image: 'https://picsum.photos/id/237/150/150' },
                { id: 6, name: '迷你單朵小香花束', price: 680, image: 'https://picsum.photos/id/237/150/150' },
                { id: 7, name: '迷你單朵小香花束', price: 680, image: 'https://picsum.photos/id/237/150/150' },
                { id: 8, name: '迷你單朵小香花束', price: 680, image: 'https://picsum.photos/id/237/150/150' },
            ],
            selectedProduct: {},  // 正在考慮但尚未決定的商品
            quantity: 1,          // 選擇的數量
            cart: [],            // 購物車中的商品
            showCart: false      // 是否顯示購物車視窗
        };
    },
    created() {
        let tmpData = JSON.parse(localStorage.getItem('cart'));
        if (tmpData) {
            this.cart = tmpData;
        }
    },
    methods: {
        // 選擇產品並設定數量
        selectProduct(product) {
            this.selectedProduct = product; // 將商品存入選中狀態
            this.quantity = 1; // 設定預設數量
        },
        // 增加數量
        increaseQuantity(item) {
            if (item) {
                item.quantity++;
            } else {
                this.quantity++;
            }
        },
        // 減少數量
        decreaseQuantity(item) {
            if (item) {
                if (item.quantity > 1) {
                    item.quantity--;
                } else {
                    this.remove(item.product.id);
                }
            } else {
                if (this.quantity > 1) {
                    this.quantity--;
                }
            }
        },
        // 將選中的產品加入購物車
        addToCart() {
            const productInCart = this.cart.find(item => item.product.id === this.selectedProduct.id);
            if (productInCart) {
                productInCart.quantity += this.quantity;
            } else {
                this.cart.push({ product: this.selectedProduct, quantity: this.quantity });
            }
            localStorage.setItem('cart', JSON.stringify(this.cart));
        },
        // 從購物車中移除產品
        remove(productId) {
            this.cart = this.cart.filter(item => item.product.id !== productId);
            if (this.cart.length > 0) {
                localStorage.setItem('cart', JSON.stringify(this.cart));
            } else {
                localStorage.removeItem('cart');
            }
        },
        // 結帳並清空購物車
        checkout() {
            alert('結帳成功！');
            this.cart = [];
            localStorage.removeItem('cart');
        }
    }
}
</script>

<style scoped>
/* 按鈕顏色 */
.btn-primary {
    background-color: #007bff;
    border-color: #007bff;
}

.btn-primary:hover {
    background-color: #0056b3;
    border-color: #004085;
}

.btn-secondary {
    background-color: #6c757d;
    border-color: #6c757d;
}

.btn-secondary:hover {
    background-color: #5a6268;
    border-color: #545b62;
}

/* 卡片圖片大小 */
.card-img-top {
    height: 200px;
    object-fit: cover;
}

/* 模態框內部樣式 */
.modal-body {
    padding: 1.5rem;
}

.modal-footer {
    padding: 1rem;
}

.list-group-item {
    display: flex;
    justify-content: space-between;
    align-items: center;
}

.input-group-sm .form-control {
    width: 80px;
}
</style>
