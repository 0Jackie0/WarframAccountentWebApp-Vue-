<template>
    <div class="contentArea">
        <span>
            <span class="tagStyle">Total Quantity: </span>{{totals.totalQuantity}}
        </span>
        <span>
            <span class="tagStyle">Total Price: </span>{{totals.totalPrice}}
        </span>
    </div>
</template>

<script lang="ts">
import Vue from 'vue'

export default Vue.extend({
    name: "ListHeader",
    props: ['itemList'],
    computed:
    {
        totals()
        {
            const totalValues = {
                totalPrice: 0,
                totalQuantity: 0
            }

            for(let itemIndex in this.itemList)
            {
                totalValues.totalPrice += this.itemList[itemIndex].eprice * this.itemList[itemIndex].quantity;
                totalValues.totalQuantity += this.itemList[itemIndex].quantity;
            }

            return totalValues
        }
    }
})
</script>

<style scoped>
.contentArea
{
    display: flex;
    flex-direction: row;

    justify-content: space-evenly;

    margin-bottom: 10px;
}
</style>