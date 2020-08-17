<template>
    <div class="listSelectionArea">
        <div class="sellFunctionArea">
            <button @click="$emit('opensellstring')">Sell Call</button>
        </div>
        <div class="filterSearchArea">
            <div class="searchArea">
                <span class="tagStyle">Search </span>
                <input type="text" placeholder="Item Name" @keyup="(event) => {this.$emit('searchfunction',event)}"/>
            </div>

            <div class="dropdownArea">
                <div class="filterSelectionArea">
                    <span class="tagStyle">Filter By: </span>
                    <select class="dropdownStyle" @change="(event) => {this.$emit('filterfunction',event)}">
                        <option value="-1">- - -</option>
                        <option v-for="filterOption in filterList" v-bind:key="filterOption.typeId" :value="filterOption.typeId">{{filterOption.typeName}}</option>
                        <!-- {props.filterList.map(filterOption => <option key={filterOption.typeId} value={filterOption.typeId}>{filterOption.typeName}</option>)} -->
                    </select>
                </div>

                <div class="orderSelectionArea">
                    <span class="tagStyle">Order By: </span>
                    <select class="dropdownStyle" @change="(event) => {this.$emit('orderfunction',event)}">
                        <option v-for="orderOption in orderList" v-bind:key="orderOption.orderId" :value="orderOption.orderId">{{orderOption.orderName}}</option>
                        <!-- {props.orderList.map(orderOption => <option key={orderOption.orderId} value={orderOption.orderId}>{orderOption.orderName}</option>)} -->
                    </select>
                </div>
            </div>
        </div>
    </div>
</template>

<script lang="ts">
import Vue from 'vue'
export default Vue.extend({
    name: "ListSelection",
    props: ['filterList','orderList'],

})
</script>

<style scoped>
.sellFunctionArea button
{
    font-size: 20px;
    font-family: 'Orbitron', sans-serif;

    padding: 5px 20px;

    background-color: white;
    color: black;
    border-radius: 12px;
    border: 2px solid #0084ff;
}
.sellFunctionArea button:hover
{
    background-color: #0084ff7c;
    color: black;
    border-radius: 12px;
    border: 2px solid white;
}

.listSelectionArea
{
    display: flex;
    flex-direction: row;
    justify-content: space-between;
}

.sellFunctionArea
{
    width: 50%;
}
.filterSearchArea
{
    display: flex;
    flex-direction: column;
    align-items: center;
    width: 50%;
}

.searchArea
{
    text-align: center;
    

    margin-bottom: 10px;
}

input[type="text"]
{
    font-size: 20px;
}

.dropdownArea
{
    display: flex;
    flex-direction: row;
    justify-content: space-evenly;


    width: 90%;
}

.filterSelectionArea
{
    width: 60%;;
}

.orderSelectionArea
{
    width: 40%;;
}
</style>