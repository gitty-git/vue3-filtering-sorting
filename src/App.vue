<template>
    <div class="search-section">
        <input placeholder="Search" class="search-input" type="text" v-model="searchFilter">
    </div>
    <table>
        <tr>
            <th>ID</th>
            <th class="sortable" @click="orderBy('name')">
                <div class="item-arrows">
                    Product name
                    <div class="arrows">
                        <div>▲</div>
                        <div>▼</div>
                    </div>
                </div>
            </th>
            <th class="sortable" @click="orderBy('quantity')">
                <div class="item-arrows">
                    Quantity
                    <div class="arrows">
                        <div>▲</div>
                        <div>▼</div>
                    </div>
                </div>
            </th>
            <th>Category</th>
            <th class="sortable" @click="orderBy('package')">
                <div class="item-arrows">
                    Package weight
                    <div class="arrows">
                        <div>▲</div>
                        <div>▼</div>
                    </div>
                </div>
            </th>
            <th class="sortable" @click="orderBy('updated_at')">
                <div class="item-arrows">
                    Updated at
                    <div class="arrows">
                        <div>▲</div>
                        <div>▼</div>
                    </div>
                </div>
            </th>
        </tr>
        <tr v-for="product in filteredProducts">
            <td>{{ product.id }}</td>
            <td>{{ product.name }}</td>
            <td>{{ product.quantity }}</td>
            <td>{{ product.category }}</td>
            <td v-if="product.package >= 1">{{ product.package }} LB</td>
            <td v-else>{{ Math.round(product.package * 2268) }} CT</td>
            <td>{{ product.updated_at }}</td>
        </tr>
    </table>
</template>

<script>
import { ref, computed } from 'vue'

export default {
    name: 'App',
    setup() {
        const products = ref([
            {id: 1, name: 'Bananas', package: 25, quantity: 15, category: 'Fruits', updated_at: '2021-01-01 01:01:01'},
            {id: 2, name: 'Diamonds', package: 0.002, quantity: 2, category: 'Jewel ', updated_at: '2021-01-02 01:01:01'},
            {id: 3, name: 'Potatoes', package: 10, quantity: 25, category: 'Vegetables', updated_at: '2021-01-02 01:01:02'},
        ])
        const orderedBy = ref({type: '', desc: false})
        const searchFilter = ref('')

        const orderBy = (type) => {
            orderedBy.value.desc = !orderedBy.value.desc
            orderedBy.value.type = type
        }

        const orderByType = computed(() => {
            if (orderedBy.value.type === 'name') {
                if (orderedBy.value.desc === true) {
                    console.log(products.value[0].product < products.value[1].product)
                    return products.value.sort((a, b) => a.name > b.name && 1 || -1)
                }
                else {
                    return products.value.sort((a, b) => b.name < a.name && -1 || 1)
                }
            }
            if (orderedBy.value.type === 'package') {
                if (orderedBy.value.desc === true) {
                    return products.value.sort((a, b) => b.package - a.package)
                }
                else {
                    return products.value.sort((a, b) => a.package - b.package)
                }
            }
            if (orderedBy.value.type === 'quantity') {
                if (orderedBy.value.desc === true) {
                    return products.value.sort((a, b) => b.quantity - a.quantity)
                }
                else {
                    return products.value.sort((a, b) => a.quantity - b.quantity)
                }
            }
            if (orderedBy.value.type === 'updated_at'){
                if (orderedBy.value.desc === true) {
                    return orderByType.value.sort((a, b) => new Date(b.updated_at) - new Date(a.updated_at))
                }
                else {
                    return orderByType.value.sort((a, b) => new Date(a.updated_at) - new Date(b.updated_at))
                }
            }
            else return products.value
        })

        const filteredProducts = computed(() => {
            return orderByType.value.filter(product => {
                const name = product.name.toLowerCase()
                const cat = product.category.toLowerCase()
                const filter = searchFilter.value.toLowerCase()
                if (name.includes(filter) || cat.includes(filter) || product.id.toString().includes(filter)) return product.name
            })
        })

        return {products, orderedBy, orderBy, searchFilter, filteredProducts, orderByType}
    }
}
</script>

<style>
* {
    margin: 0;
    padding: 0;
}
table {
    font-size: 16px;
    width: 100%;
    border-collapse: collapse;
}
th {
    padding: 20px 25px;
    text-align: left;
    background: #F9F9F9;
}
td {
    padding: 25px;
    text-align: left;
    border-bottom: 1px solid #F3F3F3;
}
input::placeholder {
    color: #c2c2c2;
}
input {
    border: 1px solid gainsboro;
    font-size: 16px;
    padding: 8px 10px;
}
.search-input {
    width: 300px;
}
.arrows {
    margin-left: 10px;
    font-size: 5px;
    color: #c2c2c2;
}
.item-arrows {
    display: flex;
}
.sortable:hover {
    cursor: pointer;
}
.search-section {
    margin: 50px 0 30px 25px;
    display: flex;
    align-items: center;
}
</style>
