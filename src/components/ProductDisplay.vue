<template lang="">

    <div :class="'productDisplay'">
        <div v-for="product in filteredProduct" :key="product.category !== categoryProduct">
            <div :class="'containerMen'" v-if="product.category === `men's clothing`">
                <div :class="'men'">
                    <img :class="'active'" :src="product.image" />
                    <div :class="'contents'">
                        <div>
                            <h3 :class="'title'">{{product.title}}</h3>
                            <p>{{product.category}}</p>
                            <div :class="'line'"></div>
                            <p>{{product.description}}</p>
                            <div :class="'line'"></div>
                            <h3 :class="'price'">${{product.price}}</h3>
                        </div>
                        <div :class="'boxButton'">
                            <button :class="'buttonPrimary'">Buy now</button>
                            <button @click="increment()" :class="'buttonSecondary'">Next product</button>
                        </div>
                    </div>
                </div>
            </div>

            <div :class="'containerWomen'" v-if="product.category === `women's clothing`">
                <div :class="'women'">
                    <img :class="'active'" :src="product.image" />
                    <div :class="'contents'">
                        <div>
                            <h3 :class="'title'">{{product.title}}</h3>
                            <p>{{product.category}}</p>
                            <div :class="'line'"></div>
                            <p>{{product.description}}</p>
                            <div :class="'line'"></div>
                            <h3 :class="'price'">${{product.price}}</h3>
                        </div>
                        <div :class="'boxButton'">
                            <button :class="'buttonPrimary'">Buy now</button>
                            <button @click="increment()" :class="'buttonSecondary'">Next product</button>
                        </div>
                    </div>
                </div>
            </div>

            <div :class="'containerOther'" v-if="product.category != `women's clothing` && product.category != `men's clothing`">
                <div :class="'other'">
                    <p>This product is unavailable to show</p>
                    <button @click="increment()" :class="'buttonSecondary'">Next product</button>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    name: 'ProductDisplay',
    props: {
        msg: String
    },
    data() {
        return {
            productsItems: [],
            count: 1,
            categoryProduct: "",
        }
    },
    methods: {
        async getData() {
            const res = await fetch("https://fakestoreapi.com/products/")
            const finalRes = await res.json()
            this.productsItems = finalRes

            console.log("sini", finalRes)
        },
        updateCategoryProduct(event) {
            this.categoryProduct = event.target.value
        },
        increment() {
            this.count++
            if (this.count > 20) {
                this.count = 1
            }
        },
    },
    computed: {
        filteredProduct() {
            const filtered = this.productsItems.filter((item) => item.id === this.count)
            return filtered.filter((data) => {
                if (data.category === "men's clothing") {
                    this.categoryProduct = "men's clothing"
                    console.log("data", data)
                    return data
                } else if (data.category === "women's clothing") {
                    this.categoryProduct = "women's clothing"
                    return data
                } else {
                    this.categoryProduct = ""
                    return data
                }
            })
        }
    },
    mounted() {
        this.getData()
    }
}
</script>

<style lang="">
</style>