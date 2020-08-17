<template>
    <div class="itemAreaUp">
        <div class="itemArea" v-on:click="$emit('openedit', item)">
            <div class="imageArea">
                <img :src="imageToshow" alt="item" />
            </div>

            <div class="itemInfoArea">
                <span>
                    <span class="tagStyle">Item Name: </span>{{item.name}}
                </span>

                <span class="quantityAndTypeLayout">
                    <span><span class="tagStyle">Quantity: </span>{{item.quantity}}</span>
                    <span class={typeStyle}>{{typeName}}</span>
                </span>

                <span>
                    <span class="tagStyle">Base Price: </span>{{item.bprice}}
                </span>

                <span>
                    <span class="tagStyle">Expected Price: </span>{{item.eprice}}
                </span>
            </div>
        </div>
        <div class="itemFunctionArea">
            <button v-on:click="$emit('addoneitem', item.itemId)"> <img :src='upImage' alt="up arrow"/> </button>
            <button v-on:click="$emit('removeoneitem', item.itemId)"> <img :src='downImage' alt="down arrow"/> </button>
        </div>
    </div>
</template>

<script lang="ts">
    import Vue from 'vue';

    export default Vue.extend({
        name: "ListContent",
        props: ["item", 'typeName'],
        data ()
        {
            const itemImage = require('../../../drawable/220px-Warframe_Cover_Art.png');
            const upImage = require('../../../drawable/up-arrow.png');
            const downImage = require('../../../drawable/down-arrow.png');

            return{
                itemImage,
                upImage,
                downImage
            }
        },
        computed:
        {
            imageToshow()
            {
                return this.item.imageString !== '' ? 'data:image/png;base64,' + this.item.imageString : this.itemImage;
            },

        },
    })
</script>

<style scoped>
.itemAreaUp
{
    display: flex;
    flex-direction: row;

    justify-content: space-between;

    background-color: #77adcc;

    height: 120px;

    margin-top: 15px;
    margin-bottom: 15px;
    margin-right: 5px;
    margin-left: 5px;
}
.itemAreaUp:hover
{
    background-color: #6c9ebb;
}

.itemArea
{
    display: flex;
    flex-direction: row;

    justify-content: flex-start;
    align-items: center;

    width: 100%;
}

.imageArea
{
    margin-left: 10px;
    background-color: #5d869e;
}
.imageArea img
{
    width: 100px;
    height: 100px;
}


.itemInfoArea
{
    width: 100%;
    display: flex;
    flex-direction: column;

    align-items: flex-start;

    margin-left: 20px;
}

.quantityAndTypeLayout
{
    width: 100%;
    display: flex;
    flex-direction: row;
    justify-content: space-between;
    align-items: center;
}
.typeBorder
{
    margin-right: 10px;
}


.itemFunctionArea
{
    display: flex;
    flex-direction: column;

    justify-content: space-evenly;

    margin-right: 10px;
}
.itemFunctionArea img
{
    width: 40px;
    height: 40px;
}

</style>